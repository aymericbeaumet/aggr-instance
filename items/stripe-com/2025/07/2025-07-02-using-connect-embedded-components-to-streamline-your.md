---
title: Using Connect embedded components to streamline your Connect onboarding flow
link: https://stripe.dev/blog/connect-embedded-components-streamline-onboarding
source: stripe-com
published: 2025-07-02T00:00:00Z
updated: 2025-07-02T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Jorge Aguirre Gonzalez
summary: Replace hosted onboarding with embedded Connect components. Keep users on your platform while collecting requirements and enabling payments..
content: extracted
html: 2025-07-02-using-connect-embedded-components-to-streamline-your.html
remote_preview:
  url: https://stripe.dev/og/blog/connect-embedded-components-streamline-onboarding.png
  alt: 'Stripe Developer Blog: Using Connect embedded components to streamline your Connect onboarding flow'
extra:
  content:interactive: true
---

Onboarding accounts and keeping them compliant and activated is one of the most important parts of your Connect platform integration. The goal is for the experience to be as streamlined as possible to maximize conversion and minimize user friction and degradation. This blog post walks you through using [Connect embedded components](https://docs.stripe.com/connect/get-started-connect-embedded-components) to achieve this.

For this walkthrough, I am using [Furever](https://www.furever.dev/), a test Connect platform for pet grooming. Let’s start by [creating a connected account](https://docs.stripe.com/api/accounts/create). In order to allow for my connected accounts to process payments, I am adding the `card_payments` and `transfers` [capabilities](https://docs.stripe.com/connect/account-capabilities) to an account fully controlled by Furever with no Stripe dashboard access, such as a Custom account. Since Furever uses [Next.js](https://nextjs.org/), I have added the [Stripe](https://www.npmjs.com/package/stripe?activeTab=readme) npm package as a dependency and have included the create account API call in the sign up flow:

```
const account = await stripe.accounts.create({
  // Account configuration
  controller: {
    stripe_dashboard: {
      type: "none",
    },
    fees: {
      payer: "application"
    },
    losses: {
      payments: "application"
    },
    requirement_collection: "application",
  },
  capabilities: {
    card_payments: {requested: true},
    transfers: {requested: true}
  },
  // Options selected in the signup UI
  country: credentials?.country || 'US',
  business_type: businessType,
  email: email,
});

console.log('Created stripe account', account.id);
```

The account ID is now created as part of the Furever signup process.

![](https://stripe.dev/images/connect-embedded-components-streamline-onboarding/image3.png)

![](https://stripe.dev/images/connect-embedded-components-streamline-onboarding/image9.png)

Now that we have a connected account, let’s first show how we would fully onboard this account without embedded components on Furever. First, I use the [create account link API](https://docs.stripe.com/api/account_links/create) and expose an API on my platform that allows me to create account links for my connected accounts. I create a new api `/create_account_link`:

```
import {getServerSession} from 'next-auth/next';
import {authOptions} from '@/lib/auth';
import {stripe} from '@/lib/stripe';

export async function POST() {
  const session = await getServerSession(authOptions);
  let stripeAccountId = session?.user?.stripeAccount?.id;

  const accountLink = await stripe.accountLinks.create({
    account: stripeAccountId,
    refresh_url: `${process.env.NEXTAUTH_URL}/home`,
    return_url: `${process.env.NEXTAUTH_URL}/home`,
    type: 'account_onboarding',
  });

  return new Response(JSON.stringify(accountLink), {
    status: 200,
    headers: {'Content-Type': 'application/json'},
  });
}
```

On my frontend, I will create a simple component that renders a button that allows the user to navigate to this account link to onboard the account to Stripe:

```

export default function Onboarding() {
  const {isLoading, error, data} = useAccountLinkCreate();

  if (error) {
    return <div>Error: {error.message}</div>;
  }
  if (isLoading || !data) {
    return <div>Loading...</div>;
  }

  const {url} = data;
  return <a href={url}>Open account link</a>;
}
```

![](https://stripe.dev/images/connect-embedded-components-streamline-onboarding/image10.png) ![](https://stripe.dev/images/connect-embedded-components-streamline-onboarding/image4.png)

We now have a fully working onboarding UX and upon completion the account is now enabled for `card_payments` and `transfers`. I can now create payments and receive payouts with this account. However, I’m not sure I like this UX as my user had to open a separate window to onboard and was required to authenticate with Stripe. I wish I could onboard my accounts from within my site in a white-labeled fashion\*.\*

Fortunately, you can do this with the [embedded onboarding component](https://docs.stripe.com/connect/supported-embedded-components/account-onboarding). First, we add a new endpoint `/create-account-session` that uses [the create account session API](https://stripe.com/docs/api/account_sessions/create) and configures it to enable the onboarding component.

```
export async function POST() {
  const session = await getServerSession(authOptions);
  let stripeAccountId = session?.user?.stripeAccount?.id;

  const accountSession = await stripe.accountSessions.create({
    account: stripeAccountId,
    components: {
      account_onboarding: {
        enabled: true,
      },
    },
  });

  return new Response(JSON.stringify(accountSession), {
    status: 200,
    headers: {'Content-Type': 'application/json'},
  });
}
```

Now, we'll use this API to initialize embedded components on my platform. I’ll need to add a dependency to the `@stripe/connect-js` and `@stripe/react-connect-js` packages.

```
yarn add @stripe/connect-js @stripe/react-connect-js
```

Now that we have these packages, I will import them and call `loadConnectAndInitialize` to initialize Connect embedded components. In this initialization, I am using the `/api/create_account_session` API we just created:

```
  
import {loadConnectAndInitialize} from '@stripe/connect-js';
import {
  ConnectAccountOnboarding,
  ConnectComponentsProvider,
} from '@stripe/react-connect-js';

export default function Onboarding() {
  const [errorMessage, setErrorMessage] = React.useState<string | null>(null);
  const [stripeConnectInstance] = React.useState(() => {
    const fetchClientSecret = async () => {
      const response = await fetch('/api/create_account_session', {
        method: 'POST',
      });
      if (!response.ok) {
        setErrorMessage('Failed to initialize Session');
        throw new Error('Failed to fetch account session');
      } else {
        const {client_secret} = await response.json();
        return client_secret;
      }
    };

    return loadConnectAndInitialize({
      publishableKey: process.env.NEXT_PUBLIC_STRIPE_PUBLIC_KEY!, // Your publishable key goes here!
      fetchClientSecret: fetchClientSecret,
    });
  });

  return (
    <>TODO: Render the component</>
  );
}
```

Next, I can then render the `ConnectAccountOnboarding` component. Let’s replace the `TODO` in the component above:

```
  ...
  return (
    <>
      {errorMessage ? (
        <div>{`Error: ${errorMessage}`}</div>
      ) : (
        <div className="container">
          <ConnectComponentsProvider connectInstance={stripeConnectInstance}>
            <ConnectAccountOnboarding
              onExit={() => {
                window.location.href = '/home'; // This is the action to perform after onboarding is exited
              }}
            />
          </ConnectComponentsProvider>
        </div>
      )}
    </>
);
```

With these changes, the embedded component renders.

![](https://stripe.dev/images/connect-embedded-components-streamline-onboarding/image7.png)

![](https://stripe.dev/images/connect-embedded-components-streamline-onboarding/image1.png)

![](https://stripe.dev/images/connect-embedded-components-streamline-onboarding/image5.png)

![](https://stripe.dev/images/connect-embedded-components-streamline-onboarding/image6.png)

We’ve successfully brought the onboarding UI within Furever and eliminated an external redirect.

Now, let’s optimize this user experience. First, the look and feel of the onboarding flow does not match the rest of the site. Let’s use the [appearance API](https://docs.stripe.com/connect/customize-connect-embedded-components) to make it match. Connect embedded components support a variety of appearance options, so I can make this look exactly like my site by simply passing in another parameter to my existing `loadConnectAndInitialize` call:

```
    return loadConnectAndInitialize({
      publishableKey: process.env.NEXT_PUBLIC_STRIPE_PUBLIC_KEY!,
      fetchClientSecret: fetchClientSecret,
      appearance: {
      variables: {
        fontFamily: 'Sohne, inherit',
        colorPrimary: '#27AE60',
        colorBackground: '#ffffff',
        colorBorder: '#D8DEE4',

        buttonPrimaryColorBackground: '#27AE60',
        buttonPrimaryColorText: '#f4f4f5',

        badgeSuccessColorBackground: '#D6FCE6',
        badgeSuccessColorText: '#1E884B',
        badgeSuccessColorBorder: '#94D5AF',

        badgeWarningColorBackground: '#FFEACC',
        badgeWarningColorText: '#C95B4D',
        badgeWarningColorBorder: '#FFD28C',

        overlayBackdropColor: 'rgba(0,0,0,0.3)',
      }
      },
    })
```

With this code, now the onboarding component fits more seamlessly into my page, as the colors, text and border radius are now matching the rest of my UI.

![](https://stripe.dev/images/connect-embedded-components-streamline-onboarding/image2.png)

As with the page hosted on Stripe, the embedded onboarding flow is requesting authentication with Stripe. This is the default behavior for embedded components. However, because I chose the `payments.losses = ‘application’` and `requirement_collection = ‘application’` options on this account (which means I as a platform [own losses](https://docs.stripe.com/api/accounts/create#create_account-controller-losses-payments) and [requirements collection](https://docs.stripe.com/api/accounts/create#create_account-controller-requirement_collection) for these connected accounts), I can disable stripe user authentication with [disable\_stripe\_user\_authentication](https://docs.stripe.com/api/account_sessions/object?api-version=2024-10-28.acacia#account_session_object-components-account_onboarding-features-disable_stripe_user_authentication). My app already has secure authentication, and I am making the explicit decision to bypass the additional [Stripe user authentication](https://docs.stripe.com/connect/get-started-connect-embedded-components?platform=web#user-authentication-in-connect-embedded-components) that comes by default with embedded components.

```
const accountSession = await stripe.accountSessions.create({
  account: 'acct_YOUR_ACCOUNT_ID',
  components: {
    account_onboarding: {
      enabled: true,
      features: {
        disable_stripe_user_authentication: true,
      },
    },
  },
});
```

Now my users don’t need to maintain a separate set of credentials with Stripe, and can jump straight into onboarding.

One more thing - I want to know exactly the steps my users go through while onboarding. Let’s integrate with the account onboarding component's `onStepChange` API so I can know exactly where my users are dropping off in the funnel:

```
<ConnectAccountOnboarding
  onExit={() => {
    console.log('Onboarding flow exited. Redirecting to home...');
    window.location.href = '/home'; // This is the action to perform after onboarding is exited
  }}
  onStepChange={(step) => {
    console.log(`User is in step ${step} of onboarding`)
    // Send this information to my platform's analytics provider...
  }}
/>
```

With this, all that is left is replacing this console log with my specific analytics tool, and now the data scientists back at Furever can link this data to all of our existing analytics.

Since this worked so well in Furever, I’d like to show you how easy it is to include other components, like the `ConnectPayments` [component](https://docs.stripe.com/connect/supported-embedded-components/payments) which would allow my connected accounts to view and manage payments from Furever.

Let’s enable the component:

```
const accountSession = await stripe.accountSessions.create({
  account: 'acct_YOUR_ACCOUNT_ID',
  components: {
    account_onboarding: {
      enabled: true,
    },
    payments: {
      enabled: true,
    },
  },
});
```

And now I can render it in the frontend, just like I did for onboarding. Generally when using multiple components, you’ll want to reuse the same `connectInstance` (see our [performance best practices](https://docs.stripe.com/connect/get-started-connect-embedded-components?platform=web#performance-best-practices)) and call `loadConnectAndInitialize` once. You can refactor this to a parent component, use react context, or any other state management solution to achieve this.

```
  return (
    <>
      {errorMessage ? (
        <div>{`Error: ${errorMessage}`}</div>
      ) : (
        <div className="container">
          <ConnectComponentsProvider connectInstance={stripeConnectInstance}>
            <ConnectPayments />
          </ConnectComponentsProvider>
        </div>
      )}
    </>
  );
```

![](https://stripe.dev/images/connect-embedded-components-streamline-onboarding/image8.png) Now I have a fully operational payment list on my site that includes refunds and dispute management, and I no longer need to build it myself.

This post has only scratched the surface of the customization that embedded components support - you can enable/disable permission-related features via [account sessions features](https://stripe.com/docs/api/account_sessions/create), or UX related features via component parameters specific to each component. We support [updating properties after initialization](https://docs.stripe.com/connect/get-started-connect-embedded-components?platform=web#update-connect-embedded-components-after-initialization) (so supporting a dark mode toggle on your app would be easy), [localization](https://docs.stripe.com/connect/get-started-connect-embedded-components?platform=web#localization), and support all frontend UI frameworks (not just React) via our [vanilla JS SDK](https://github.com/stripe/connect-js).

There are many [GA and preview components](https://docs.stripe.com/connect/supported-embedded-components), and more are on the way. Adding another component to your site is as easy as updating the Account Session creation call, and integrating the new component in the frontend - each new component respects the same theming parameters to fit seamlessly into your own UI. From account [onboarding](https://docs.stripe.com/connect/supported-embedded-components/account-onboarding) and account [management](https://docs.stripe.com/connect/supported-embedded-components/account-management), to [payments](https://docs.stripe.com/connect/supported-embedded-components/payments), [payouts](https://docs.stripe.com/connect/supported-embedded-components/payouts), [capital financing offers](https://docs.corp.stripe.com/connect/supported-embedded-components/capital-financing-application) and [instant payouts](https://docs.stripe.com/connect/supported-embedded-components/balances). These components are a great way to quickly implement deep, maintainable and high quality payments functionality into your site and avoid external redirects.

For more Stripe learning resources, subscribe to our [YouTube channel](https://www.youtube.com/stripedevelopers).
