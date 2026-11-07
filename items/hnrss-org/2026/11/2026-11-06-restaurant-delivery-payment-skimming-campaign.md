---
title: Restaurant Delivery Payment-Skimming Campaign
link: https://www.infohawk.com/news/2026-11-05-scamwich-restaurant-skimming
source: hnrss-org
published: 2026-11-06T19:19:27Z
updated: 2026-11-06T19:19:27Z
first_seen: 2026-11-07T10:39:02.057754401Z
authors:
- fiej
content: extracted
html: 2026-11-06-restaurant-delivery-payment-skimming-campaign.html
remote_preview:
  url: https://www.infohawk.com/_app/immutable/assets/2026-11-05-scamwich-restaurant-skimming.CsokS_Fm.png
  alt: 'Scamwich: Inside a Restaurant Delivery Payment-Skimming Campaign — InfoHawk News'
---

![Copycat homepages: a Jersey Mike's look-alike (left) and a Dave's Hot Chicken look-alike (right)](https://www.infohawk.com/_app/immutable/assets/hero.BVcKpwn9.jpg)\
 Source: jarrysaymlckess\[.\]delivery and dawechlckhort\[.\]delivery

When customers order food online, they expect a familiar experience: find their restaurant, choose their meal, and pay. Increasingly, criminals are exploiting that familiarity by creating convincing clones of restaurant ordering websites, right down to the original site's underlying code.

In early November 2026, InfoHawk researchers investigated a collection of suspicious websites that appear to be part of one or more coordinated restaurant impersonation and payment-skimming operations.

Across two research passes, we identified 820 domains impersonating 35 restaurant brands. Of those, 808 use the `.delivery` top-level domain, and the rest use `.com` or `.shop`.

The scale is concerning, but what makes these sites particularly dangerous is how convincing they can be.

## These aren't ordinary fake websites

The investigation began with a suspicious domain: `mlckessejersstey[.]delivery`.

The site impersonated Jersey Mike's, the popular sandwich chain, and reproduced its online ordering experience.

Rather than simply displaying a crude imitation of the restaurant's website, the attackers used a more sophisticated approach: reverse-proxying the legitimate site while replacing the checkout payment form with a malicious one.

A reverse proxy can retrieve content from a legitimate website and present it to visitors under a different domain. This allows an attacker-controlled site to display genuine pages and assets while selectively modifying the parts of the experience that matter to the attacker.

In this case, the malicious checkout requested credit card information and also asked customers for the SMS verification code sent by their bank.

This is a particularly dangerous combination. Capturing a credit card number is valuable to criminals; obtaining a one-time verification code could also help them authorize fraudulent transactions, depending on how the victim's bank uses that code.

For consumers, the experience can appear nearly indistinguishable from ordering directly from the restaurant. The site looks familiar, the menu and ordering flow can behave normally, and the fraudulent payment form appears at the moment a customer expects to pay.

Importantly, this does not require the legitimate restaurant's website to have been hacked. The attackers can operate on infrastructure they control while using the authentic restaurant website as the source of the content they display.

## Impersonated brands

Mostly `.delivery` typosquats, plus a few look-alike `.com` and `.shop` names, found across two research passes. "Seen" is the range of registration or scan dates we have for a brand's domains, all in 2026. Open a brand to see its domains.

Brand cloned Domains Seen

Jimmy John's 404 Mar 3 – Oct 21

- jijmmyjomsandwich\[.\]delivery
- jilmmyjjsendwich\[.\]delivery
- jilmmyjohsendwich\[.\]delivery
- jilmmyjomsendwich\[.\]delivery
- jimijunes\[.\]delivery
- jimjjonesand\[.\]delivery
- jimlyjoosandwich\[.\]delivery
- jimmijones\[.\]delivery
- jimmijons\[.\]delivery
- jimmjjohsendwic\[.\]delivery
- jimmjjohsendwich\[.\]delivery
- jimmjonnsandwichers\[.\]delivery
- jimmjonsandwichers\[.\]delivery
- jimmjonsandwiches\[.\]delivery
- jimmjosandwichers\[.\]delivery
- jimmjosandwiches\[.\]delivery
- jimmjosandwicthers\[.\]delivery
- jimmlysandwichdel\[.\]delivery
- jimmsandwichers\[.\]delivery
- jimmujohsandwisch\[.\]delivery
- jimmyjiosandwichs\[.\]delivery
- jimmyjjons\[.\]delivery
- jimmyjjosandwichs\[.\]delivery
- jimmyjjsandwichs\[.\]delivery
- jimmyjjsandwitch\[.\]delivery
- jimmyjohsandvich\[.\]delivery
- jimmyjohsandwich\[.\]delivery
- jimmyjonsendwich\[.\]delivery
- jimmyjosandwitch\[.\]delivery
- jimmyjosendwiches\[.\]delivery
- jimmysandwich\[.\]delivery
- jimmysandwichjj\[.\]delivery
- jimmysandwichjjo\[.\]delivery
- jimmysandwichjlon\[.\]delivery
- jimmysandwichjon\[.\]delivery
- jimmysandwitch\[.\]delivery
- jimmysendwichjon\[.\]delivery
- jimmywhojo\[.\]delivery
- jimyjjohsandwichs\[.\]delivery
- jimyjjomsandwichs\[.\]delivery
- jimyjjosandwiches\[.\]delivery
- jimyjjosandwicth\[.\]delivery
- jimyjjosendwiches\[.\]delivery
- jimyjjosendwicthes\[.\]delivery
- jimyjohsandwichs\[.\]delivery
- jjimmyssandwich\[.\]delivery
- jjlmmydelsendwich\[.\]delivery
- jjmjohnsandch\[.\]delivery
- jjmmjjohsendwich\[.\]delivery
- jjmmjonnsandwichas\[.\]delivery
- jjmmjonnsandwiches\[.\]delivery
- jjmmjonnsandwichs\[.\]delivery
- jjmmjonnsandwicths\[.\]delivery
- jjmmjonsandw\[.\]delivery
- jjmmjonsandwicheres\[.\]delivery
- jjmmjonsandwichers\[.\]delivery
- jjmmjonsandwiches\[.\]delivery
- jjmmjonsandwichs\[.\]delivery
- jjmmjonysandwicths\[.\]delivery
- jjmmjornsandwicths\[.\]delivery
- jjmmyjohdelandwach\[.\]delivery
- jjmmyjohdelinsandwah\[.\]delivery
- jjmmyjohdelirsandwch\[.\]delivery
- jjmmyjohdelirsandwech\[.\]delivery
- jjmmyjohdelirsendw\[.\]delivery
- jjmmyjohdelisandw\[.\]delivery
- jjmmyjohdelisandwach\[.\]delivery
- jjmmyjohdelisandwah\[.\]delivery
- jjmmyjohdelisandwe\[.\]delivery
- jjmmyjohdelisandwec\[.\]delivery
- jjmmyjohdelisandwech\[.\]delivery
- jjmmyjohdelisandweh\[.\]delivery
- jjmmyjohdelisendw\[.\]delivery
- jjmmyjohdelitsandwch\[.\]delivery
- jjmmyjohdelivsandw\[.\]delivery
- jjmmyjohdelivsandwach\[.\]delivery
- jjmmyjohdelivsandwanh\[.\]delivery
- jjmmyjohdelivsandwech\[.\]delivery
- jjmmyjohdelivsendw\[.\]delivery
- jjmmyjohdelivsendwich\[.\]delivery
- jjmmyjohdelivssandwech\[.\]delivery
- jjmmyjohdelivvsendw\[.\]delivery
- jjmmyjohdeliwsendw\[.\]delivery
- jjmmyjohdelyvsandw\[.\]delivery
- jjmmyjohedelivsandwch\[.\]delivery
- jjmmyjohnsanbwanh\[.\]delivery
- jjmmyjohnsandwanh\[.\]delivery
- jjmmyjohsand\[.\]delivery
- jjmmyjohsandwanh\[.\]delivery
- jjmmyjohsandwans\[.\]delivery
- jjmmyjohsandwens\[.\]delivery
- jjmmyjohsandwihes\[.\]delivery
- jjmmyjohsandwihs\[.\]delivery
- jjmmyjohsandwinds\[.\]delivery
- jjmmyjohsandwins\[.\]delivery
- jjmmyjohsandwit\[.\]delivery
- jjmmyjohsandwiths\[.\]delivery
- jjmmyjohsandwits\[.\]delivery
- jjmmyjohsandwons\[.\]delivery
- jjmmyjohsendvitches\[.\]delivery
- jjmmyjohsendvitchs\[.\]delivery
- jjmmyjohsendwech\[.\]delivery
- jjmmyjohsendwich\[.\]delivery
- jjmmyjohsendwihs\[.\]delivery
- jjmmyjohsendwins\[.\]delivery
- jjmmyjohsendwitchs\[.\]delivery
- jjmmyjohsendwiths\[.\]delivery
- jjmmyjomsandwich\[.\]delivery
- jjmmyjones\[.\]delivery
- jjmmyjonesanb\[.\]delivery
- jjmmyjonesanbwitch\[.\]delivery
- jjmmyjonesandwihes\[.\]delivery
- jjmmyjonesandwit\[.\]delivery
- jjmmyjonesandwitch\[.\]delivery
- jjmmyjonesdely\[.\]delivery
- jjmmyjonessndw\[.\]delivery
- jjmmyjonhes\[.\]delivery
- jjmmyjonhesanbwihes\[.\]delivery
- jjmmyjonhesandwihes\[.\]delivery
- jjmmyjonhessandw\[.\]delivery
- jjmmyjonhessanw\[.\]delivery
- jjmmyjonsanbwanh\[.\]delivery
- jjmmyjonsand\[.\]delivery
- jjmmyjonsandwinds\[.\]delivery
- jjmmyjonsandwirds\[.\]delivery
- jjmmyjonsandwlrds\[.\]delivery
- jjmmyjonsanw\[.\]delivery
- jjmmyjonsanwanh\[.\]delivery
- jjmmyjonsanwatnh\[.\]delivery
- jjmmyjoomsandwich\[.\]delivery
- jjmmyjoonsandwich\[.\]delivery
- jjmmyjorhsandwit\[.\]delivery
- jjmmyjosandchv\[.\]delivery
- jjmmyjosandchw\[.\]delivery
- jjmmyjosandwitch\[.\]delivery
- jjmmyjosendwich\[.\]delivery
- jjmmysanbwihes\[.\]delivery
- jjmmysandwichs\[.\]delivery
- jjmmyyjjons\[.\]delivery
- jjmmyyjjos\[.\]delivery
- jjmmyyjohsendwich\[.\]delivery
- jjohjimmsendwich\[.\]delivery
- jjohjimmsendwiches\[.\]delivery
- jjohjjmmysendwiches\[.\]delivery
- jjojlmmydelsendwicher\[.\]delivery
- jjummyjohsandwitch\[.\]delivery
- jjummyjosandwitch\[.\]delivery
- jjummyjosandwitchs\[.\]delivery
- jjymmjohsandwichs\[.\]delivery
- jlimmjoohsandwiches\[.\]delivery
- jlimmysaandwiches\[.\]delivery
- jlimysaandwiches\[.\]delivery
- jlmmjohsanbit\[.\]delivery
- jlmmjohsanbwes\[.\]delivery
- jlmmjohsanbwichs\[.\]delivery
- jlmmjohsanbwihs\[.\]delivery
- jlmmjohsanbwis\[.\]delivery
- jlmmjohsandwichs\[.\]delivery
- jlmmjonnsandwicths\[.\]delivery
- jlmmjornesandwicths\[.\]delivery
- jlmmjornsandwicths\[.\]delivery
- jlmmujo\[.\]delivery
- jlmmyandjonhsandwitchs\[.\]delivery
- jlmmydelsendched\[.\]delivery
- jlmmydelsendwched\[.\]delivery
- jlmmydelsendwlched\[.\]delivery
- jlmmydelsenwched\[.\]delivery
- jlmmydelysendwich\[.\]delivery
- jlmmyjjdelivsendwlch\[.\]delivery
- jlmmyjjdsendwlch\[.\]delivery
- jlmmyjjohsandwich\[.\]delivery
- jlmmyjjosandwich\[.\]delivery
- jlmmyjjosendwich\[.\]delivery
- jlmmyjjsandwich\[.\]delivery
- jlmmyjjsandwitchs\[.\]delivery
- jlmmyjjsendwlch\[.\]delivery
- jlmmyjlosandwich\[.\]delivery
- jlmmyjohbsandw\[.\]delivery
- jlmmyjohdels\[.\]delivery
- jlmmyjohdelsenwched\[.\]delivery
- jlmmyjohedelivsanbwch\[.\]delivery
- jlmmyjohedelivsandwch\[.\]delivery
- jlmmyjohedely\[.\]delivery
- jlmmyjohedelys\[.\]delivery
- jlmmyjohehs\[.\]delivery
- jlmmyjohehsanbvach\[.\]delivery
- jlmmyjohehsanbvatch\[.\]delivery
- jlmmyjohehsandvatch\[.\]delivery
- jlmmyjohne\[.\]delivery
- jlmmyjohnesandw\[.\]delivery
- jlmmyjohnsandw\[.\]delivery
- jlmmyjohnsandwictchs\[.\]delivery
- jlmmyjohnssandwicths\[.\]delivery
- jlmmyjohsandw\[.\]delivery
- jlmmyjohsandwich\[.\]delivery
- jlmmyjohsandwicths\[.\]delivery
- jlmmyjohsandwitch\[.\]delivery
- jlmmyjohsandwiths\[.\]delivery
- jlmmyjohsendwitch\[.\]delivery
- jlmmyjonesanbwitches\[.\]delivery
- jlmmyjonesandwes\[.\]delivery
- jlmmyjonesandwitches\[.\]delivery
- jlmmyjonhes\[.\]delivery
- jlmmyjonhsand\[.\]delivery
- jlmmyjonhsandw\[.\]delivery
- jlmmyjonhsandwe\[.\]delivery
- jlmmyjonhssanwit\[.\]delivery
- jlmmyjonnsandwicths\[.\]delivery
- jlmmyjonnsandwiths\[.\]delivery
- jlmmyjonnsandwlths\[.\]delivery
- jlmmyjonsanbwits\[.\]delivery
- jlmmyjonsanbwts\[.\]delivery
- jlmmyjonsand\[.\]delivery
- jlmmyjonsandwic\[.\]delivery
- jlmmyjonsandwich\[.\]delivery
- jlmmyjonsandwicths\[.\]delivery
- jlmmyjonsandwitches\[.\]delivery
- jlmmyjonsandwits\[.\]delivery
- jlmmyjonsendches\[.\]delivery
- jlmmyjonsendwches\[.\]delivery
- jlmmyjonsendwitches\[.\]delivery
- jlmmyjonsendwtches\[.\]delivery
- jlmmyjonssandwicths\[.\]delivery
- jlmmyjonssanitch\[.\]delivery
- jlmmyjonssanltch\[.\]delivery
- jlmmyjonssanwit\[.\]delivery
- jlmmyjonssendches\[.\]delivery
- jlmmyjornsandwicths\[.\]delivery
- jlmmyjorsandwicths\[.\]delivery
- jlmmyjorsandwictks\[.\]delivery
- jlmmyjorsandwits\[.\]delivery
- jlmmyjosanbwch\[.\]delivery
- jlmmyjosanbwit\[.\]delivery
- jlmmyjosanbwits\[.\]delivery
- jlmmyjosandwich\[.\]delivery
- jlmmyjosandwits\[.\]delivery
- jlmmyjosanwit\[.\]delivery
- jlmmyjsanbitch\[.\]delivery
- jlmmyjsanitch\[.\]delivery
- jlmmysanbwits\[.\]delivery
- jlmmysandwich\[.\]delivery
- jlmmysandwick\[.\]delivery
- jlmmysandwictchs\[.\]delivery
- jlmmysendwich\[.\]delivery
- jlmmysendwlch\[.\]delivery
- jlmmytojoh\[.\]delivery
- jlmmyydelsendwlched\[.\]delivery
- jlmysandwick\[.\]delivery
- jlojlmmydelsendwicher\[.\]delivery
- jlojlmmydelsendwiches\[.\]delivery
- jlojlmmydelysendwiches\[.\]delivery
- jmmyjjsandwisch\[.\]delivery
- jmmysondwichejonh\[.\]delivery
- jmyyjohsandwich\[.\]delivery
- jmyyjohsendwich\[.\]delivery
- jmyyjohssandwich\[.\]delivery
- johjimmsandw\[.\]delivery
- johjjmmydelsendwiches\[.\]delivery
- johjjmmysendwiches\[.\]delivery
- johnejimmydely\[.\]delivery
- johnejimmys\[.\]delivery
- johnejimmysdely\[.\]delivery
- johnejlmmys\[.\]delivery
- johnejlmmysenbwich\[.\]delivery
- johnejlmmysenbwiched\[.\]delivery
- johnejlmmysendwiched\[.\]delivery
- jojjmmydelsendwiches\[.\]delivery
- jojjmmysendwiches\[.\]delivery
- jojjmydelsendwiches\[.\]delivery
- jojjmydelsendwichs\[.\]delivery
- jojjmydelysendwichs\[.\]delivery
- jojlmmydelsendwich\[.\]delivery
- jojlmmydelsendwiched\[.\]delivery
- jojlmmydelsendwicher\[.\]delivery
- jojlmmydelsendwiches\[.\]delivery
- jojlmmyydelsendwiched\[.\]delivery
- jojlmmyydelsendwlched\[.\]delivery
- jonejlmmysendwiched\[.\]delivery
- jonhejjmmyssadwch\[.\]delivery
- jonhejlmmys\[.\]delivery
- jonhejlmmysenbwichd\[.\]delivery
- jonhejlmmysenbwiched\[.\]delivery
- jonhejlmmysendwiched\[.\]delivery
- jonhejlmmyssadwch\[.\]delivery
- jonjlmmydelsendwiched\[.\]delivery
- jonjlmmydelsendwicher\[.\]delivery
- jonjlmmydelssendwiched\[.\]delivery
- jonnjlmmydelssendwiched\[.\]delivery
- jonsjlmmydelsendwich\[.\]delivery
- jonyjlmmydelsendwich\[.\]delivery
- joohjimmsendwich\[.\]delivery
- jummjohsandvach\[.\]delivery
- jummjohsandvatch\[.\]delivery
- jummjohsandvech\[.\]delivery
- jummjohsandwich\[.\]delivery
- jummjohsandwiched\[.\]delivery
- jummjohsandwiches\[.\]delivery
- jummjohsandwitch\[.\]delivery
- jummjohsandwitchar\[.\]delivery
- jummjohsandwitcher\[.\]delivery
- jummjohsandwitches\[.\]delivery
- jummjohysandwich\[.\]delivery
- jummujohsandwiched\[.\]delivery
- jummujohsandwirched\[.\]delivery
- jummydelivesandmet\[.\]delivery
- jummydelivesandwitch\[.\]delivery
- jummydelivsendwich\[.\]delivery
- jummyjjdelisendwich\[.\]delivery
- jummyjjdelivsendwich\[.\]delivery
- jummyjjomsandwitch\[.\]delivery
- jummyjjosendwiches\[.\]delivery
- jummyjnhsenbvicher\[.\]delivery
- jummyjnhsencher\[.\]delivery
- jummyjodelivesandmet\[.\]delivery
- jummyjohehsandvatch\[.\]delivery
- jummyjohhsandvatch\[.\]delivery
- jummyjohnsanbich\[.\]delivery
- jummyjohnsandiches\[.\]delivery
- jummyjohnsandichs\[.\]delivery
- jummyjohnsandwis\[.\]delivery
- jummyjohnsandws\[.\]delivery
- jummyjohnsanichs\[.\]delivery
- jummyjohsahdwich\[.\]delivery
- jummyjohsanbich\[.\]delivery
- jummyjohsanbtch\[.\]delivery
- jummyjohsanbwich\[.\]delivery
- jummyjohsanbwtch\[.\]delivery
- jummyjohsandvatch\[.\]delivery
- jummyjohsandvetch\[.\]delivery
- jummyjohsandvich\[.\]delivery
- jummyjohsandvitch\[.\]delivery
- jummyjohsandvltch\[.\]delivery
- jummyjohsandvtch\[.\]delivery
- jummyjohsandvttch\[.\]delivery
- jummyjohsandwch\[.\]delivery
- jummyjohsandwetch\[.\]delivery
- jummyjohsandwich\[.\]delivery
- jummyjohsandwitch\[.\]delivery
- jummyjohsandwtch\[.\]delivery
- jummyjohsantch\[.\]delivery
- jummyjohsendvicher\[.\]delivery
- jummyjohsendwich\[.\]delivery
- jummyjohsendwiche\[.\]delivery
- jummyjohsendwiched\[.\]delivery
- jummyjohsendwicher\[.\]delivery
- jummyjohsendwicth\[.\]delivery
- jummyjohsendwitch\[.\]delivery
- jummyjohsondwich\[.\]delivery
- jummyjomsandwitch\[.\]delivery
- jummyjomsendvilch\[.\]delivery
- jummyjomsendvitch\[.\]delivery
- jummyjomsendwitch\[.\]delivery
- jummyjonesendvicher\[.\]delivery
- jummyjonhesenbvicher\[.\]delivery
- jummyjonhesendvicher\[.\]delivery
- jummyjonhs\[.\]delivery
- jummyjonhsanbich\[.\]delivery
- jummyjonhsanbvatch\[.\]delivery
- jummyjonhsand\[.\]delivery
- jummyjonhsandichs\[.\]delivery
- jummyjonhsandvatch\[.\]delivery
- jummyjonhsandw\[.\]delivery
- jummyjonhsandwy\[.\]delivery
- jummyjonhsendwar\[.\]delivery
- jummyjonhsendwer\[.\]delivery
- jummyjonhssandwy\[.\]delivery
- jummyjoohsandwch\[.\]delivery
- jummyjoosandwtch\[.\]delivery
- jummyjorsandvatchs\[.\]delivery
- jummyjorsandvetch\[.\]delivery
- jummyjorsandvltch\[.\]delivery
- jummyjorsandvotch\[.\]delivery
- jummyjorysandvatchs\[.\]delivery
- jummyjosandmet\[.\]delivery
- jummyjosandvartch\[.\]delivery
- jummyjosandvat\[.\]delivery
- jummyjosandvatch\[.\]delivery
- jummyjosandvet\[.\]delivery
- jummyjosandvetch\[.\]delivery
- jummyjosandvit\[.\]delivery
- jummyjosandvitch\[.\]delivery
- jummyjosandvltch\[.\]delivery
- jummyjosandwhat\[.\]delivery
- jummyjosandwhit\[.\]delivery
- jummyjosandwit\[.\]delivery
- jummyjosandwitch\[.\]delivery
- jummyjosandwtch\[.\]delivery
- jummyjossandvlch\[.\]delivery
- jummyjossandvltch\[.\]delivery
- jummyjsanbvatch\[.\]delivery
- jummyjsanbvtch\[.\]delivery
- jummyjsandwtch\[.\]delivery
- jummyjsanwtch\[.\]delivery
- jumyjohnesandwch\[.\]delivery
- jymmjohsandwiches\[.\]delivery
- jymmjohsandwichs\[.\]delivery
- jymmjohsandwitches\[.\]delivery
- jymmyjohsandwitches\[.\]delivery
- jymmyjonesandwitches\[.\]delivery
- sandwichedordr\[.\]delivery
- sandwichordr\[.\]delivery
- sandwicthedjjon\[.\]delivery
- sandwicthjjoh\[.\]delivery
- sandwicthjjon\[.\]delivery

Firehouse Subs 121 Jul 19 – Nov 1

- filresubeshoysse\[.\]delivery
- filresubshoysse\[.\]delivery
- firedhoussubs\[.\]delivery
- firehoossesulbs\[.\]delivery
- firehossedulbs\[.\]delivery
- firehossesubs\[.\]delivery
- firehossesulbs\[.\]delivery
- firehossetulbs\[.\]delivery
- firehosseulbs\[.\]delivery
- firehossubsup\[.\]delivery
- firehossulbs\[.\]delivery
- firehousechicken\[.\]shop
- firehousedeals\[.\]delivery
- firehousego\[.\]delivery
- firehousesubes\[.\]delivery
- firehousesubs-us\[.\]com
- firehousesubs\[.\]delivery
- firehousesubss\[.\]delivery
- firehousrsubes\[.\]delivery
- firehoussesub\[.\]delivery
- firehoussesube\[.\]delivery
- firehoussesubes\[.\]delivery
- firehoussesulbes\[.\]delivery
- firehoussesulbs\[.\]delivery
- firehoussub\[.\]delivery
- firehoussuber\[.\]delivery
- firehoussubes\[.\]delivery
- firehoussubpick\[.\]delivery
- firehoussubs\[.\]delivery
- firehoyssesubs\[.\]delivery
- firehoyssesulbs\[.\]delivery
- fireshoussubs\[.\]delivery
- firesubeshouses\[.\]delivery
- firesubeshouss\[.\]delivery
- firesubeshousse\[.\]delivery
- firesubeshoussed\[.\]delivery
- firesubeshousser\[.\]delivery
- firesubeshoussers\[.\]delivery
- firesubesthouses\[.\]delivery
- firesubhousse\[.\]delivery
- firesubhoussed\[.\]delivery
- firesubseshousse\[.\]delivery
- firesubseshoussed\[.\]delivery
- firesubshousse\[.\]delivery
- firesubshousser\[.\]delivery
- firesupeshouss\[.\]delivery
- firethoussesubs\[.\]delivery
- firetohome\[.\]delivery
- firtehousto\[.\]delivery
- fitresubshousse\[.\]delivery
- fitresubshoysse\[.\]delivery
- flirehousser\[.\]delivery
- flredhoussesubs\[.\]delivery
- flredhoustesubs\[.\]delivery
- flreghoustesubs\[.\]delivery
- flrehausedsybses\[.\]delivery
- flrehomesubs\[.\]delivery
- flrehoossesulbs\[.\]delivery
- flrehoousesulbs\[.\]delivery
- flrehousersub\[.\]delivery
- flrehousersube\[.\]delivery
- flrehousersubes\[.\]delivery
- flrehousersubs\[.\]delivery
- flrehousessubs\[.\]delivery
- flrehousesub\[.\]delivery
- flrehousesube\[.\]delivery
- flrehousesubes\[.\]delivery
- flrehousesubest\[.\]delivery
- flrehousesubs\[.\]delivery
- flrehousesubup\[.\]delivery
- flrehousetsubs\[.\]delivery
- flrehoussersubs\[.\]delivery
- flrehoussesubs\[.\]delivery
- flrehousybses\[.\]delivery
- flrehoyssesubs\[.\]delivery
- flrehoyssesulbs\[.\]delivery
- flreshoussubs\[.\]delivery
- flresubhoussed\[.\]delivery
- flresubshous\[.\]delivery
- flresubshouse\[.\]delivery
- flresubshousse\[.\]delivery
- flresubshoust\[.\]delivery
- flresubshoutse\[.\]delivery
- flresubshouze\[.\]delivery
- flresubshouzer\[.\]delivery
- flresubshoysse\[.\]delivery
- flresybseshaused\[.\]delivery
- flrethoyssesubs\[.\]delivery
- hosesfirenow\[.\]delivery
- housetoflre\[.\]delivery
- subefirerhouses\[.\]delivery
- subefirerhouss\[.\]delivery
- subeflrerhouss\[.\]delivery
- subesfirerhoysse\[.\]delivery
- subfireehouse\[.\]delivery
- subfireehouser\[.\]delivery
- subfireehouses\[.\]delivery
- subfirehousse\[.\]delivery
- subfirehoussed\[.\]delivery
- subfirerhouses\[.\]delivery
- subfirerhouset\[.\]delivery
- subfireshousd\[.\]delivery
- subfireshouse\[.\]delivery
- subfireshoused\[.\]delivery
- subfireshousef\[.\]delivery
- subfireshousepd\[.\]delivery
- subfireshouser\[.\]delivery
- subfirethouset\[.\]delivery
- subflreehouses\[.\]delivery
- subflreshoused\[.\]delivery
- subflrethouset\[.\]delivery
- subsfireehouse\[.\]delivery
- subsfireehoysse\[.\]delivery
- subsfirehousse\[.\]delivery
- subsfirehoussed\[.\]delivery
- subsfirerhouses\[.\]delivery
- subsfirerhoysse\[.\]delivery
- subsfireshouse\[.\]delivery
- subsflrehoussed\[.\]delivery
- subsflrehoysse\[.\]delivery
- subsflrerhoysse\[.\]delivery

Jersey Mike's 80 Jul 18 – Sep 20

- jarrsysdaydmlces\[.\]delivery
- jarrsysdaymlces\[.\]delivery
- jarrsysdaymlcess\[.\]delivery
- jarrsysdaysmlces\[.\]delivery
- jarrylaymlcess\[.\]delivery
- jarrysaylmlckess\[.\]delivery
- jarrysaymlcess\[.\]delivery
- jarrysaymlckess\[.\]delivery
- jarrysdaymlcess\[.\]delivery
- jarrysslaymkcess\[.\]delivery
- jarrysslaymlcess\[.\]delivery
- jarrysstaymlcess\[.\]delivery
- jarsaydelimlcess\[.\]delivery
- jarsaydelmlcess\[.\]delivery
- jarsaydelymlcess\[.\]delivery
- jarsaymlcess\[.\]delivery
- jerrjyseymlctes\[.\]delivery
- jerrlysaymlctes\[.\]delivery
- jerrlysaytmlctes\[.\]delivery
- jerrlyseymlctes\[.\]delivery
- jerrseylmlctes\[.\]delivery
- jerrseymlctes\[.\]delivery
- jerrseyrmlctes\[.\]delivery
- jerrsseymlkess\[.\]delivery
- jerrsteymlctes\[.\]delivery
- jerrydelivemlces\[.\]delivery
- jerrydelivmlces\[.\]delivery
- jerrydelivmlcess\[.\]delivery
- jerrydelivrmlces\[.\]delivery
- jerrysdelymlcess\[.\]delivery
- jerryseymlctes\[.\]delivery
- jerryseymlktes\[.\]delivery
- jerryssaymkcress\[.\]delivery
- jerryssaymlcess\[.\]delivery
- jerrystaymlctess\[.\]delivery
- jerrystaymlktes\[.\]delivery
- jerrystayrmlctess\[.\]delivery
- jerrysteymlctes\[.\]delivery
- jerrysteymlktes\[.\]delivery
- jerseymikes-delivery\[.\]com
- jersmlckessjerssy\[.\]delivery
- jerssaymlkess\[.\]delivery
- jerssaysmlkess\[.\]delivery
- jerssaysrmlkess\[.\]delivery
- jerssdaymlces\[.\]delivery
- jerssdaymlcess\[.\]delivery
- jerssdaymlkces\[.\]delivery
- jersseymlkess\[.\]delivery
- jersylmlclkessjerssey\[.\]delivery
- jersymlckessjerssy\[.\]delivery
- jersymlckesstjerssy\[.\]delivery
- jerymlckessjenssy\[.\]delivery
- jerymlckessjerssey\[.\]delivery
- jerymlcketssjerssey\[.\]delivery
- jerymlclkessjerssey\[.\]delivery
- jerymlkejerssey\[.\]delivery
- jerysdaymlcess\[.\]delivery
- jeryssaymlcess\[.\]delivery
- jeryssaymlkess\[.\]delivery
- mcklesjerssay\[.\]delivery
- mlckejerssey\[.\]delivery
- mlckesjerssay\[.\]delivery
- mlckessejersstey\[.\]delivery
- mlckessjersay\[.\]delivery
- mlckessjerssay\[.\]delivery
- mlckessjerssays\[.\]delivery
- mlckessjerssey\[.\]delivery
- mlckessjersseys\[.\]delivery
- mlckessjersstey\[.\]delivery
- mlckessljersay\[.\]delivery
- mlckesstjersay\[.\]delivery
- mlcketssjersstey\[.\]delivery
- mlcklesjerssay\[.\]delivery
- mlcklessjerssay\[.\]delivery
- mlclkessjerssey\[.\]delivery
- mlclkesstjerssey\[.\]delivery
- mlkessjerssay\[.\]delivery
- mlklesjerssay\[.\]delivery
- mlklesrjerssay\[.\]delivery
- mlklessjerssay\[.\]delivery

Dave's Hot Chicken 67 Aug 4 – Oct 7

- davechckenshot\[.\]delivery
- davechickenedhost\[.\]delivery
- davechlckanhot\[.\]delivery
- davechlckenhot\[.\]delivery
- davechlckenshot\[.\]delivery
- davehostchlcken\[.\]delivery
- davehotchcken\[.\]delivery
- davehottchcken\[.\]delivery
- davehottchlcken\[.\]delivery
- davehotterchlcken\[.\]delivery
- davehotteschickan\[.\]delivery
- daverschlcken\[.\]delivery
- daveschickanshot\[.\]delivery
- daveschickenhot\[.\]delivery
- daveschlckens\[.\]delivery
- daveshostchlckan\[.\]delivery
- daveshostchlcken\[.\]delivery
- daveshotchlckened\[.\]delivery
- daveshotschickan\[.\]delivery
- daveshottchlcken\[.\]delivery
- daveshottchlckens\[.\]delivery
- daveshottechlckens\[.\]delivery
- daveshotterchlcken\[.\]delivery
- daveshottschlckens\[.\]delivery
- davesschlcken\[.\]delivery
- davesshostchlcken\[.\]delivery
- davizohotchikenezo\[.\]delivery
- davshotchlcken\[.\]delivery
- davsschlcken\[.\]delivery
- davsshotchkken\[.\]delivery
- davsshotchlcken\[.\]delivery
- davsshotchlkken\[.\]delivery
- dawechlckenhort\[.\]delivery
- dawechlckhort\[.\]delivery
- dawehortchlcken\[.\]delivery
- dawehotchlckan\[.\]delivery
- dawehottchlcken\[.\]delivery
- dawershiortchlckan\[.\]delivery
- dawershiortchlcken\[.\]delivery
- dawershortchlcken\[.\]delivery
- dawersshiortchlcken\[.\]delivery
- daweschlckanhort\[.\]delivery
- daweschlckeneshort\[.\]delivery
- daweschlckenhort\[.\]delivery
- daweschlckenshort\[.\]delivery
- daweschlckenshott\[.\]delivery
- daweshortchickans\[.\]delivery
- daweshotterchlckan\[.\]delivery
- daweshotterchlcken\[.\]delivery
- daweshotteschlcken\[.\]delivery
- dawshiortchlcken\[.\]delivery
- dawshiortchlckens\[.\]delivery
- dawshorteschlckan\[.\]delivery
- dawshorteschlcken\[.\]delivery
- dawshortschlcken\[.\]delivery
- dawshotchlcken\[.\]delivery
- dawshottchlckan\[.\]delivery
- dawshottchlcken\[.\]delivery
- hotchlckandave\[.\]delivery
- hotchlckandaves\[.\]delivery
- hotchlckendave\[.\]delivery
- hotchlckendaves\[.\]delivery
- hotchlckensdaves\[.\]delivery
- hotschckensdaves\[.\]delivery
- hotschlckandavs\[.\]delivery
- hotschlckendavs\[.\]delivery
- hotschlckensdaves\[.\]delivery

Marco's Pizza 24 Apr 19 – Aug 20

- macospizz\[.\]delivery
- macroatepizz\[.\]delivery
- macroeatepizz\[.\]delivery
- macroeatepizze\[.\]delivery
- macrospizz\[.\]delivery
- macrospizzaz\[.\]delivery
- macsordevipisa\[.\]delivery
- macsordevipissa\[.\]delivery
- macsordevipissap\[.\]delivery
- macsorsdevipissa\[.\]delivery
- marcaspizass\[.\]delivery
- marccos\[.\]delivery
- marcidelivypizza\[.\]delivery
- marcsespizzars\[.\]delivery
- marcsopizaz\[.\]delivery
- marcsorpizaz\[.\]delivery
- marocspizzarias\[.\]delivery
- marocssopizzaria\[.\]delivery
- maroocspizzaria\[.\]delivery
- marrcocpizaria\[.\]delivery
- marrocpizzaria\[.\]delivery
- pizazmarcsoc\[.\]delivery
- pizazmarcsocs\[.\]delivery
- pizzarmarcas\[.\]delivery

Waffle House 19 Aug 4 – Sep 26

- 0rder-waffle\[.\]delivery
- 0rderwaffle\[.\]delivery
- home-waffle\[.\]delivery
- home-wafh0us\[.\]delivery
- homewafh0uss\[.\]delivery
- w00ffleh0use\[.\]delivery
- w0ffle-c0ffe\[.\]delivery
- w0ffle-caf3e\[.\]delivery
- w0ffleh00use\[.\]delivery
- w0ffleh0use\[.\]delivery
- w0flehuse\[.\]delivery
- wa0fleh0use\[.\]delivery
- wafflehouse\[.\]delivery
- wafklleshh0use\[.\]delivery
- waflehuse\[.\]delivery
- waflles-caffe7\[.\]delivery
- wafllless-coffe7\[.\]delivery
- wafloho0se\[.\]delivery
- wafolhose\[.\]delivery

Tropical Smoothie Cafe 15 Aug 4 – Sep 25

- 0rder-troppical\[.\]delivery
- caffe-sm0thtropicca\[.\]delivery
- caffes-tropicl\[.\]delivery
- caffes-trropicl\[.\]delivery
- caffesm0thtropicl\[.\]delivery
- caffetr0pical\[.\]delivery
- cafffe-sm0thtropic\[.\]delivery
- h0me-tropicalcaffe\[.\]delivery
- order-tr0picalca\[.\]delivery
- tr00pical-order\[.\]delivery
- tr00ppicalcaffee\[.\]delivery
- tr0pica-caffe\[.\]delivery
- tr0pical-smoth\[.\]delivery
- tr0ppicalcaffee\[.\]delivery
- tro0pism0th\[.\]delivery

Torchy's Tacos 12 Sep 27 – Oct 20

- t0rchystac0s\[.\]delivery
- tacostorchser\[.\]delivery
- torchikstacses\[.\]delivery
- torchostaces\[.\]delivery
- torchosttaces\[.\]delivery
- torchosttacs\[.\]delivery
- torchunstacross\[.\]delivery
- torchunstacrosst\[.\]delivery
- torchyosttaces\[.\]delivery
- torchystac0ss\[.\]delivery
- tortchystaces\[.\]delivery
- tostchysttacoes\[.\]delivery

Miller's Ale House 10 Sep 6 – Sep 27

- m1lershase\[.\]delivery
- mil0rs0l0h00se\[.\]delivery
- mil3rsh0use\[.\]delivery
- milerh0se\[.\]delivery
- mill0rs0l0h00se\[.\]delivery
- mill0rs0leh0use\[.\]delivery
- millrs0l0h0se\[.\]delivery
- milresl0h0se\[.\]delivery
- mllleraleh00use\[.\]delivery
- mllleraleh0use\[.\]delivery

Show 26 more brands Hide 26 brands 68

Cold Stone Creamery 9 Aug 7 – Sep 26

- c00ldst0neecre0m\[.\]delivery
- c0ldst00neecre0m\[.\]delivery
- c0ldst0neecr0m\[.\]delivery
- c0ldst0neecre0m\[.\]delivery
- co0ldst0neecrem\[.\]delivery
- cold-creamst0nne\[.\]delivery
- coldsst0neccrame\[.\]delivery
- coldst0necrame\[.\]delivery
- home-coldst0nerame\[.\]delivery

Buffalo Wild Wings 6 Jun 12 – Aug 23

- baffalowildeswinds\[.\]delivery
- bufatowildeswlng\[.\]delivery
- buff0lowilldwings\[.\]delivery
- buffatowildeswins\[.\]delivery
- bufwildeswiing\[.\]delivery
- order-buff0lowilldw\[.\]delivery

Chipotle 6 Aug 9 – Sep 7

- chlpolledelivery\[.\]com
- chlpolleldelivery\[.\]com
- chlpotedeliverys\[.\]com
- chlpoteldeliverys\[.\]com
- chlpotelrdeliverys\[.\]com
- chlpotelrtdeliverys\[.\]com

Auntie Anne's 4 Sep 21 – Sep 27

- aunienn3s\[.\]delivery
- auntieatnners\[.\]delivery
- aunttleannes\[.\]delivery
- ayntleannes\[.\]delivery

Bob Evans 4 Aug 6 – Sep 6

- b0bevans\[.\]delivery
- b0bveans\[.\]delivery
- b0evans\[.\]delivery
- b0obveans\[.\]delivery

Graeter's 4 Aug 23

- gr0et0rs\[.\]delivery
- gr0etars\[.\]delivery
- graet0rs\[.\]delivery
- grat0rs\[.\]delivery

Penn Station 4 Oct 18

- panonstatlon\[.\]delivery
- pensinstaton\[.\]delivery
- pentistatlon\[.\]delivery
- statinpention\[.\]delivery

Red Lobster 4 Jul 29

- radlobstery\[.\]delivery
- redllobsteer\[.\]delivery
- reedlbster\[.\]delivery
- retlodster\[.\]delivery

Arby's 3 Sep 14

- arbyis\[.\]delivery
- arbyssend\[.\]delivery
- arrbyscom\[.\]delivery

Chicken Salad Chick 3 Sep 4 – Sep 6

- chick0ns0l0dchick\[.\]delivery
- chick0nsl0dchick\[.\]delivery
- chickns0l0dchick\[.\]delivery

Little Caesars 3 May 2 – Jul 12

- litllecaersors\[.\]delivery
- litllekaersars\[.\]delivery
- llttlecaeresars\[.\]delivery

McAlister's Deli 2 Jul 15

- macallisersdel\[.\]delivery
- mccaliseterdeliv\[.\]delivery

Panda Express 2 Aug 17

- pandaexpressdelivery\[.\]com
- pndaexpresess\[.\]delivery

Shipley Donuts 2 Oct 2 – Oct 7

- shipl0yd0nuts\[.\]delivery
- shipll0yd0nutss\[.\]delivery

Carl's Jr. 1 Aug 10

- carlsjrc0ffe\[.\]delivery

El Pollo Loco 1 Aug 17

- elp0l0l0c0\[.\]delivery

Five Guys 1 Sep 9

- fiveguys-us\[.\]com

Hungry Howie's 1 Aug 4

- hunngryc0fe\[.\]delivery

Insomnia Cookies 1 Aug 6

- ins0mniak00k\[.\]delivery

Mellow Mushroom 1 Oct 21

- mallowsmustrooms\[.\]delivery

Panera Bread 1 Aug 20

- panerrabred\[.\]delivery

Popeyes 1 Aug 22

- popyyes\[.\]delivery

Sheetz 1 Sep 1

- sh00tz\[.\]delivery

Tim Hortons 1 Jul 27

- tlmshotrots\[.\]delivery

Wawa 1 Aug 15

- 0rder-wo0wa\[.\]delivery

Wingstop 1 Sep 6

- wingstop-delivery\[.\]com

## How the attack works

Each clone is a **reverse proxy**: a server that sits between the victim and the real restaurant site, fetches every page from the real site as it is requested, edits it on the way through, and passes it on. Menus, prices, store hours, and the cart are live and correct, because they come from the real site. Only the payment step is replaced. Described from mlckessejersstey\[.\]delivery (Jersey Mike's); we believe the clones of the other brands run the same kit.

1. ### The victim arrives from a search ad

   A Google Search ad imitates the restaurant's listing and shows a look-alike domain. The domain is behind Cloudflare, so its real server is hidden, and Cloudflare gives it a valid certificate automatically (Google Trust Services here), so the browser shows "Connection is secure".

2. ### Every page is fetched live from the real site

   The proxy forwards each request to the restaurant's own servers, adjusting the host name and cookies, and relays the answer. The real site's headers come through (on Jersey Mike's, `x-powered-by: Nuxt` and Amazon CloudFront headers), which is how we could tell which paths came from the real site and which from the scammer's server.

3. ### Addresses are rewritten to keep the victim on the fake domain

   The proxy replaces the brand's own host names in pages, scripts, and redirects with the fake domain, and maps each backend to a path on it, such as `/__bapi__/` or `/__payments__/`. Store lookup, menus, login, and cart all work, because those calls go back through the proxy to the real APIs. Even the brand's own bot-detection script still runs, with its endpoints rewritten.

4. ### Three scripts are injected into every page

   **A script blocker** patches `HTMLScriptElement.prototype.src` so that listed scripts never load (on Jersey Mike's, the OneTrust cookie banner and Branch, which could send phone users to the app or the real domain). **`_jmu()`** finds the real checkout payment frame and points it at `/js/index.html` on the fake domain; a MutationObserver puts the fake one back whenever the page restores the real one. **`_chkT()`** reads the order total from the "Place order" button every half second and saves it, so the fake form shows the right amount.

5. ### The card form is the scammer's

   A few paths are answered by the scammer's server instead of the real site: `/js/*` (the card form and its scripts) and `/payment/socket.io`. The form looks like part of checkout. On submit it collects every field (card number, expiry, CVV, name, phone) and sends it, with the site name, to the operator over a live socket.io connection.

6. ### An operator gets past the bank's check live

   We believe a person watching a control panel uses the card elsewhere and, when the bank challenges that payment, tells the victim's page what to show next: "card declined, try another", a page asking for the SMS code (`/js/confirmation/`), an "approve in your bank app" page (`/js/app/`), or "invalid code, try again". We believe the code the victim types is what lets the operator's charge through 3-D Secure.

7. ### The victim is sent away

   When the operator marks it a success, the page sets a cookie `valid=77777` lasting until 2038 and sends the victim to another chain's real site (`href.li/?https://www.jimmyjohns.com/`; href.li hides where they came from). No order reaches the restaurant: the real payment step never runs, so there is no confirmation email and no food. We believe the victim's card is then charged for whatever the operator bought.

**Why this design.** The clone never goes out of date, the victim builds a real order with a real total before anything looks wrong, and one kit works against any restaurant by changing the target site, the domain, and a few path rules. The scammer's server does very little work, which makes it cheap to move. Some variants skip the payment wording and only pin the frame's address, re-setting it every time the page changes it.

## Distribution of the Delivery Scam

Searching Google's ad archive (Ads Transparency Center) for the domains in our first sweep found **at least 47 Google Search text ads** between July 7, 2026, and October 20, 2026, run under **at least 46 separate advertiser accounts** with at least 43 different, unrelated-looking business names (construction firms, a reptile rescue, a watch company). The ads below are a sub-sample: we searched only for the domains in our first sweep, so there are likely more. Each ad imitates a restaurant's search listing and shows a look-alike domain. Most ran for 1–4 days, usually starting days after the domain was registered.

**Status:** On November 4, 2026, 367 of the 820 domains still served a site, so they could be getting traffic through means other than ads. None of the ads below were still running on November 3, 2026, but this scam may also be running through other ads, or other means. Google's public Ads Transparency pages now show "Can't find ad" for the creatives and "No ads found" for the domains we checked, which suggests Google has taken them down; the "Ads Transparency page" links below will likely not load. The images and text here come from InfoHawk's own capture of Google's archive, made on or before November 3, 2026. Of the 47 ads below, 41 have Google's rendered image; the rest show the text from Google's ad preview. Domains in the image ads were read from the image and may contain small reading errors.

Jimmy John's 20 Jul 7 – Sep 18

jjmmyjohsendwitchs\[.\]delivery Jul 7 – Jul 13 · 7 days

CORE LIGHT CONSTRUCTION

AR02849962528929218561 · CR04959296421980274689

![Google search ad for jjmmyjohdeliwsendw[.]delivery: Order Sandwiches for Delivery - Order Sandwiches for Pick Up](https://www.infohawk.com/_app/immutable/assets/ad-01.BeYipQzy.png)

jjmmyjohdeliwsendw\[.\]delivery Jul 10 – Jul 15 · 6 days

Bewildered Books LLC

AR08814498649288474625 · CR11136522483933380609

![Google search ad for jojlmmydelsendwiches[.]delivery: Sandwich Shop](https://www.infohawk.com/_app/immutable/assets/ad-02.BPfieGiN.png)

jojlmmydelsendwiches\[.\]delivery Jul 14 – Jul 14 · 1 day

Nuisance Alley LLC

AR13428097817948192769 · CR10060196945047060481

![Google search ad for jimmmyjohsandwichs[.]delivery: Order Sandwiches for Pick Up - Sandwich Shop](https://www.infohawk.com/_app/immutable/assets/ad-03.CHM51efs.png)

jimmmyjohsandwichs\[.\]delivery Jul 21 – Jul 22 · 2 days

Nadman Inc.

AR15690002555809562625 · CR16110921995201282049

![Google search ad for jjmmyjonsandwinds[.]delivery: Order Sandwiches for Pick Up - Sandwich Shop](https://www.infohawk.com/_app/immutable/assets/ad-04.CzWIwswI.png)

jjmmyjonsandwinds\[.\]delivery Jul 22 – Jul 22 · 1 day

DTec IO LLC

AR07717795586526674945 · CR13395814369850294273

![Google search ad for jummyjohehsandvatch[.]delivery: Order Sandwiches for Pick Up - Order Sandwiches for Delivery](https://www.infohawk.com/_app/immutable/assets/ad-05.su-ah1PD.png)

jummyjohehsandvatch\[.\]delivery Aug 8 – Aug 9 · 2 days

Malkani & Co LLC

AR12407670625584283649 · CR17623871435497799681

![Google search ad for jummyjosandvartch[.]delivery: Order Sandwiches for Pick Up - Sandwich Shop](https://www.infohawk.com/_app/immutable/assets/ad-06.R9FIaMew.png)

jummyjosandvartch\[.\]delivery Aug 15 – Aug 21 · 7 days

Daphne Digital LLC

AR11508891338133209089 · CR01145629208483987457

![Google search ad for jummyjohehsandvatch[.]delivery: Order Sandwiches for Pick Up - Order Sandwiches for Delivery](https://www.infohawk.com/_app/immutable/assets/ad-07.COxRAvGa.png)

jummyjohehsandvatch\[.\]delivery Aug 15 – Aug 16 · 2 days

QENN Watch Co., LLC

AR07253183204314578945 · CR13246651873647132673

![Google search ad for jjmmyjonesandwihes[.]delivery: Sandwich Shop - Order Sandwiches for Delivery](https://www.infohawk.com/_app/immutable/assets/ad-08.CALcmnzo.png)

jjmmyjonesandwihes\[.\]delivery Aug 16 – Aug 20 · 5 days

Harari Homes

AR05990869088837042177 · CR10379671730808422401

![Google search ad for jjmmyjonesandwit[.]delivery: Order Sandwiches for Pick Up - Order Sandwiches for Delivery](https://www.infohawk.com/_app/immutable/assets/ad-09.-CvEcLE-.png)

jjmmyjonesandwit\[.\]delivery Aug 16 – Aug 18 · 3 days

LiKa Consulting LLC

AR10490528066533588993 · CR14853240671967379457

![Google search ad for jymmyjonesandwitches[.]delivery: Order Sandwiches for Delivery - Order Sandwiches for Pick Up](https://www.infohawk.com/_app/immutable/assets/ad-10.COHiVbKz.png)

jymmyjonesandwitches\[.\]delivery Aug 16 – Sep 18 · 34 days

Mini Builders Play LLC

AR11242066254351040513 · CR01279831199723814913

![Google search ad for jjmmyjohsandwanh[.]delivery: Sandwich Shop - Order Sandwiches for Pick Up](https://www.infohawk.com/_app/immutable/assets/ad-11.BYD1xTMX.png)

jjmmyjohsandwanh\[.\]delivery Aug 16 – Aug 19 · 4 days

Mini Builders Play LLC

AR15917893383096369153 · CR02320760847971909633

jlmmyydelsendwlched\[.\]delivery Aug 17 – Aug 19 · 3 days

Harari Homes

AR16455044395683545089 · CR18177073943460970497

jjymmjohsandwichs\[.\]delivery Aug 17 – Aug 24 · 8 days

The Ripple Effect ISR LLC

AR01755295211946246145 · CR01201071807436685313

jummyjohnsanbich\[.\]delivery Aug 20 – Aug 25 · 6 days

Clutch N Burn MC Incorporated

AR05244679750210813953 · CR00819138851342647297

![Google search ad for jimmmyjohsandwichs[.]delivery: Order Sandwiches for Pick Up - Sandwich Shop](https://www.infohawk.com/_app/immutable/assets/ad-12.BWyiNwuy.png)

jimmmyjohsandwichs\[.\]delivery Aug 23 – Aug 24 · 2 days

Laidir, LLC

AR07868477332921516033 · CR10097855905493155841

![Google search ad for jummyjohehsandvatch[.]delivery: Order Sandwiches for Pick Up - Order Sandwiches for Delivery](https://www.infohawk.com/_app/immutable/assets/ad-13.WcsY9ztw.png)

jummyjohehsandvatch\[.\]delivery Aug 23 – Aug 24 · 2 days

Raaga Sudha Sabha

AR13572145523497893889 · CR09509555611981840385

![Google search ad for jjmmyjohdeliwsendw[.]delivery: Order Sandwiches for Delivery - Order Sandwiches for Pick Up](https://www.infohawk.com/_app/immutable/assets/ad-14.BEBgMAWg.png)

jjmmyjohdeliwsendw\[.\]delivery Aug 23 – Aug 27 · 5 days

Qik Studio LLC

AR16686384665824591873 · CR10847014750148100097

![Google search ad for jjlmmydelsendwich[.]delivery: Sandwich Shop - Order Sandwiches for Pick Up](https://www.infohawk.com/_app/immutable/assets/ad-15.5aNBJXXa.png)

jjlmmydelsendwich\[.\]delivery Aug 23 – Aug 25 · 3 days

K.H. Wadsworth LLC

AR09354524472465424385 · CR10155677022974640129

![Google search ad for jimmmyjohsandwichs[.]delivery: Order Sandwiches for Pick Up - Sandwich Shop](https://www.infohawk.com/_app/immutable/assets/ad-16.DkzW236b.png)

jimmmyjohsandwichs\[.\]delivery Aug 26 – Sep 7 · 13 days

Brand Name Swain LLC

AR04233505085633593345 · CR02940724052706197505

Firehouse Subs 12 Jul 28 – Sep 30

![Google search ad for flresubshoutse[.]delivery: Firehouse Subs](https://www.infohawk.com/_app/immutable/assets/ad-17.BAcFtumh.png)

flresubshoutse\[.\]delivery Jul 28 – Aug 1 · 5 days

KVLASH SANJOSE LLC

AR02160684599351443457 · CR07199522089076785153

![Google search ad for flrehoossesulbs[.]delivery: Firehouse Subs - Menu Items](https://www.infohawk.com/_app/immutable/assets/ad-18.BZ9Si116.png)

flrehoossesulbs\[.\]delivery Jul 29 – Aug 6 · 9 days

Brian Wobst LLC

AR02707656449797914625 · CR07567261999323676673

![Google search ad for flreghoustesubs[.]delivery: Firehouse Subs](https://www.infohawk.com/_app/immutable/assets/ad-19.B2a-BnWu.png)

flreghoustesubs\[.\]delivery Jul 29 – Aug 6 · 9 days

The Way, The Light, The Woods LLC

AR02616458557343662081 · CR06669091291558576129

![Google search ad for flresubshoutse[.]delivery: Firehouse Subs](https://www.infohawk.com/_app/immutable/assets/ad-20.BNf3XA-s.png)

flresubshoutse\[.\]delivery Jul 30 – Jul 31 · 2 days

226 E. MAUNA LOA, LLC

AR15778061342566514689 · CR09176454441178497025

![Google search ad for firehoyssesubs[.]delivery: Firehouse Subs - Menu With Calories and Nutrit](https://www.infohawk.com/_app/immutable/assets/ad-21.DTq9TbQv.png)

firehoyssesubs\[.\]delivery Aug 1 – Aug 7 · 7 days

H&S RT GROUP INC

AR02475483574476734465 · CR01043450493403136001

![Google search ad for firehoyssesulbs[.]delivery: Firehouse Subs - Subs, Sub Sandwiches, Salads](https://www.infohawk.com/_app/immutable/assets/ad-22.DEy1i6_s.png)

firehoyssesulbs\[.\]delivery Aug 12 – Aug 14 · 3 days

NurseWise Consulting LLC

AR03995389624842190849 · CR07844799624895266817

![Google search ad for firesubhousse[.]delivery: Firehouse Subs - Menu Items](https://www.infohawk.com/_app/immutable/assets/ad-23.C0bPA0xM.png)

firesubhousse\[.\]delivery Aug 12 – Aug 14 · 3 days

Daphne Digital LLC

AR10130774458995048449 · CR15993869636575690753

![Google search ad for firehoyssesulbs[.]delivery: Firehouse Subs - Menu - Order Online](https://www.infohawk.com/_app/immutable/assets/ad-24.AeU7yFZk.png)

firehoyssesulbs\[.\]delivery Aug 17 – Aug 20 · 4 days

BT Design LLC

AR11784688436798029825 · CR14045936079022850049

![Google search ad for subsfirehousse[.]delivery: Firehouse Subs - Order Online](https://www.infohawk.com/_app/immutable/assets/ad-25.DruYhxrC.png)

subsfirehousse\[.\]delivery Aug 17 – Aug 20 · 4 days

ecstudio LLC

AR08391226117574361089 · CR05861628918695460865

![Google search ad for firehoussesulbs[.]delivery: Firehouse Subs - Order Online](https://www.infohawk.com/_app/immutable/assets/ad-26.9l-BgEuM.png)

firehoussesulbs\[.\]delivery Aug 20 – Aug 22 · 3 days

1 Stop Renovations Ga Inc

AR00408178788275322881 · CR15694935239849672705

![Google search ad for subfireehouse[.]delivery: Firehouse Subs - Subs, Sub Sandwiches, Salads - Order Online](https://www.infohawk.com/_app/immutable/assets/ad-27.D0lZEjcD.png)

subfireehouse\[.\]delivery Sep 6 – Sep 10 · 5 days

Reptile Ron Rescue Inc.

AR14244803097871253505 · CR03304396319686131713

![Google search ad for firehouseshousss[.]delivery: Firehouse Subs - Subs, Sub Sandwiches, Salads](https://www.infohawk.com/_app/immutable/assets/ad-28.CYlP6KA5.png)

firehouseshousss\[.\]delivery Sep 16 – Sep 30 · 15 days

Corotos LLC

AR15735555047670415361 · CR09306568826143375361

Dave's Hot Chicken 11 Aug 18 – Oct 20

davehottchlcken\[.\]delivery Aug 18 – Aug 19 · 2 days

CIVELA LLC

AR00066323031013720065 · CR00241954620388147201

![Google search ad for davechckenshot[.]delivery: Dave's Hot Chicken: Home - Menu](https://www.infohawk.com/_app/immutable/assets/ad-29.C6nBMIC7.png)

davechckenshot\[.\]delivery Aug 19 – Aug 21 · 3 days

B SUPPLY 82 INC

AR10774238075505606657 · CR17901127311214772225

![Google search ad for davehotterchlcken[.]delivery: Dave's Menu - Dave's Hot Chicken: Home](https://www.infohawk.com/_app/immutable/assets/ad-30.bKGHuFTD.png)

davehotterchlcken\[.\]delivery Aug 19 – Aug 19 · 1 day

Alyx De Jong LLC

AR16845090373201035265 · CR07594639563977392129

![Google search ad for davehottchcken[.]delivery: Dave's Hot Chicken: Home - Menu](https://www.infohawk.com/_app/immutable/assets/ad-31.BS7xsDH_.png)

davehottchcken\[.\]delivery Aug 21 – Aug 22 · 2 days

AMARTA NUSANTARA LLC

AR08623808835854794753 · CR05281757481322676225

![Google search ad for dawechlckhort[.]delivery: Menu - Dave's Hot Chicken](https://www.infohawk.com/_app/immutable/assets/ad-32.B7y0Por7.png)

dawechlckhort\[.\]delivery Aug 23 – Aug 27 · 5 days

Nana's Purse

AR07310446594522873857 · CR14867537346785443841

![Google search ad for davshotchlcken[.]delivery: Dave's Menu - Dave's Hot Chicken: Home - Locations](https://www.infohawk.com/_app/immutable/assets/ad-33.CAcgJ8Nk.png)

davshotchlcken\[.\]delivery Aug 23 – Aug 24 · 2 days

TIDAL IX, LLC

AR01140515105025294337 · CR10882133151539265537

dawershiortchlckan\[.\]delivery Aug 24 – Aug 27 · 4 days

Viora Creamery LLC

AR10311301898770055169 · CR02308823175351238657

![Google search ad for hotchlckandaves[.]delivery: Dave's Hot Chicken - Dave's Menu](https://www.infohawk.com/_app/immutable/assets/ad-34.BDf-gms1.png)

hotchlckandaves\[.\]delivery Aug 24 – Oct 1 · 39 days

Snippity Snap

AR17965169465486213121 · CR17915540809143287809

![Google search ad for dawshottchlckan[.]delivery: Dave's Hot Chicken - Dave's Hot Chicken: Home - Dave's Menu](https://www.infohawk.com/_app/immutable/assets/ad-35.C2tpF3rz.png)

dawshottchlckan\[.\]delivery Aug 26 – Oct 20 · 56 days

Uva Nest, LLC

AR04661487787722145793 · CR07219698402324381697

![Google search ad for davesschlcken[.]delivery: Locations - Dave's Hot Chicken](https://www.infohawk.com/_app/immutable/assets/ad-36.ALx4REIR.png)

davesschlcken\[.\]delivery Aug 26 – Sep 9 · 15 days

Jasmine Center, LLC

AR00036290970412646401 · CR13757673267278118913

![Google search ad for davesschlcken[.]delivery: Menu - Dave's Hot Chicken - Dave's Hot Chicken: Home](https://www.infohawk.com/_app/immutable/assets/ad-37.B_AJ1bNd.png)

davesschlcken\[.\]delivery Sep 2 – Sep 9 · 8 days

Jasmine Center, LLC

AR00036290970412646401 · CR14781743004225896449

Jersey Mike's 4 Jul 21 – Jul 31

![Google search ad for jeryssaymlcess[.]delivery: Jersey Mike's Subs - Menu - Order Online](https://www.infohawk.com/_app/immutable/assets/ad-38.CYkX1pIy.png)

jeryssaymlcess\[.\]delivery Jul 21 – Jul 21 · 1 day

Conleys Concessions LLC

AR15563338816289767425 · CR03137520416281591809

![Google search ad for mlckessjerssey[.]delivery: Jersey Mike's Subs - Jersey Mike's Delivery - Catering Menu](https://www.infohawk.com/_app/immutable/assets/ad-39.BpjzgdC6.png)

mlckessjerssey\[.\]delivery Jul 24 – Jul 29 · 6 days

LECO Advisory LLC

AR10554591661281771521 · CR07935840836942561281

![Google search ad for jerrysdelymlcess[.]delivery: Jersey Mike's Delivery](https://www.infohawk.com/_app/immutable/assets/ad-40.CIq-mWWl.png)

jerrysdelymlcess\[.\]delivery Jul 28 – Jul 29 · 2 days

Orise Group, Inc.

AR07188953583310602241 · CR05321661781951643649

![Google search ad for jerymlclkessjerssey[.]delivery: Order Jersey Mike's Online - Jersey Mike's Delivery - Catering Menu](https://www.infohawk.com/_app/immutable/assets/ad-41.DXlz-V4E.png)

jerymlclkessjerssey\[.\]delivery Jul 30 – Jul 31 · 2 days

ONSETOLOGY LLC

AR14583974122615734273 · CR09920322435880583169

## Hosting

Most of the clones sit behind Cloudflare (AS13335), which hides the real servers: 761 of the 820 domains use Cloudflare nameservers, 11 use other nameservers, and 48 have none. Another cluster of 68 domains is tied to an Amazon Web Services (AWS) server. Some of its sites are served straight from that server, in Frankfurt (3.79.192.101) through my-ndns.com nameservers, and its pages carry a different injected script (/static/\<name>-fix.js). It uses the same cloning technique, but we found nothing that shows the same people run it, so these sites may come from more than one operator using the same kit.

In our first sweep of 255 domains, 251 were registered at Dynadot, spread across 111 Cloudflare nameserver pairs.

| Server          | Network                                                                                                                | Domains hosted                                                                                                                                                                                | Status Nov 3                                                                    |
| --------------- | ---------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| 196.251.107.226 | AS214351 FEMO IT Solutions (GB); block named "internet-security", listed in the Seychelles; abuse: abuse@rentdedic.com | The 46 Dave's Hot Chicken clones from our first sweep, all 6 Chipotle, and all 4 Red Lobster, plus a BJ's Restaurants clone as its default site. Runs a live card panel (/payment/socket.io). | Live                                                                            |
| 63.143.44.131   | AS46475 Limestone Networks (US); abuse: abuse@limestonenetworks.com                                                    | wingstop-delivery\[.\]com, fiveguys-us\[.\]com                                                                                                                                                | Not answering                                                                   |
| 70.33.249.47    | AS13768 Aptum / Peer1 (CA); abuse: abuse@aptum.com                                                                     | jerseymikes-delivery\[.\]com                                                                                                                                                                  | Not answering                                                                   |
| 38.180.12.63    | Cogent address space announced by AS10929 eStruxture (CA); abuse: abuse@cogentco.com                                   | firehousesubs-us\[.\]com                                                                                                                                                                      | Not answering                                                                   |
| 3.79.192.101    | AWS EC2, Frankfurt; abuse: trustandsafety@support.aws.com                                                              | 7 domains from the AWS cluster, including two Waffle House clones (w0flehuse\[.\]delivery, wafklleshh0use\[.\]delivery)                                                                       | Returned 502 on November 3; the two Waffle House clones were live on November 4 |

## What this means for restaurant brands

These clones don't need to break into a restaurant's systems. They borrow the real site, page by page, and insert the attack only at the moment a customer pays. That makes them hard to spot from the outside, but it also means the brand's own servers see every page the clone serves. Brands and their ordering providers that watch for their site loading under someone else's domain, or for checkout sessions that never reach the real payment provider, are well placed to catch these sites early.

## Is your brand being cloned? *Let's find out.*

InfoHawk tracks look-alike domains, the ads that send shoppers to them, and the infrastructure behind them. If you want help detecting attacks like this against your brand, or protecting your website from this cloning technique, get in touch.

[Talk to InfoHawk](https://www.infohawk.com/contact?type=contact)
