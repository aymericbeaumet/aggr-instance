---
title: Python Workers are now generally available
link: https://blog.cloudflare.com/python-workers-ga/
source: hnrss-org-frontpage
published: 2026-09-21T13:38:19Z
updated: 2026-09-21T13:38:19Z
first_seen: 2026-09-21T21:01:27.933476777Z
authors:
- torutofu
summary: 'Article URL: https://blog.cloudflare.com/python-workers-ga/ Comments URL: https://news.ycombinator.com/item?id=49787142 Points: 146 # Comments: 21'
content: extracted
html: 2026-09-21-python-workers-are-now-generally-available.html
preview:
  file: 2026-09-21-python-workers-are-now-generally-available.preview-a34a43c46fac.webp
  width: 256
  height: 134
  color: '#1e0f3d'
images:
- source: https://blog.cloudflare.com/_emdash/api/media/file/01M31JM2B4K28B7C67PCT5PB2R.01M31JM35NW9707QDHV86F68M9.png
  original:
    file: 2026-09-21-python-workers-are-now-generally-available.image-983a2d84b34b.png
    width: 1200
    height: 628
  color: '#070033'
- source: https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M2V34B98AFHX986KP70WF54Y.01M2V34CGNFYXX59FZ2MPKEKWK.png&w=1430&h=650&f=webp&fit=cover&position=center
  original:
    file: 2026-09-21-python-workers-are-now-generally-available.image-4ee148303ccd.webp
    width: 1430
    height: 650
  variants:
  - file: 2026-09-21-python-workers-are-now-generally-available.image-230ff9ccc128.webp
    width: 320
    height: 145
  color: '#fefefe'
- source: https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M2V34BJ3JC9ESH8HE7K14QS3.01M2V34CN9240HX0Y61C6R57TE.png&w=1430&h=276&f=webp&fit=cover&position=center
  original:
    file: 2026-09-21-python-workers-are-now-generally-available.image-d5e516fca901.webp
    width: 1430
    height: 275
  variants:
  - file: 2026-09-21-python-workers-are-now-generally-available.image-b26bc4b25b35.webp
    width: 320
    height: 62
  color: '#fefefe'
- source: https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M2V34F3DBEWD2N6YV1ZS9XV1.01M2V34G4DP7AAFRGZTS13W1EF.png&w=1430&h=276&f=webp&fit=cover&position=center
  original:
    file: 2026-09-21-python-workers-are-now-generally-available.image-cd16210feee4.webp
    width: 1430
    height: 275
  variants:
  - file: 2026-09-21-python-workers-are-now-generally-available.image-7b73e5046173.webp
    width: 320
    height: 62
  color: '#fefefe'
- source: https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M2V34FR0NPET1PKA1YGE6QS7.01M2V34GSM4JXGX4DMA1H393CS.png&w=1430&h=728&f=webp&fit=cover&position=center
  original:
    file: 2026-09-21-python-workers-are-now-generally-available.image-c84f9b9fa000.webp
    width: 1430
    height: 728
  variants:
  - file: 2026-09-21-python-workers-are-now-generally-available.image-414c808e317a.webp
    width: 320
    height: 163
  color: '#fefefe'
- source: https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M2V34GW5EXQCQW27RV0EWYEH.01M2V34J33FCM8TBEM9TXQ098T.png&w=1430&h=1028&f=webp&fit=cover&position=center
  original:
    file: 2026-09-21-python-workers-are-now-generally-available.image-590bcab488a6.webp
    width: 1330
    height: 956
  variants:
  - file: 2026-09-21-python-workers-are-now-generally-available.image-ce31d78ebf04.webp
    width: 320
    height: 230
  color: '#fdfdfd'
---

We introduced Python Workers two years ago, providing a way to [run Python applications](https://blog.cloudflare.com/python-workers/) in the Cloudflare Workers runtime. Our goal was to make it as simple to write Workers in Python as it is in TypeScript, and to make the ecosystem of Python packages and frameworks “just work”.

Today, Python Workers are now generally available (GA).

What does GA mean? It means Python is now a first-class, fully supported language on the Cloudflare Developer Platform. You can bring the Python code, libraries, and design patterns you already know and connect them seamlessly to Workers AI, R2, D1, Hyperdrive, Durable Objects, Queues, Workflows, and the rest of the Cloudflare platform. You can also run popular Python frameworks like FastAPI, Django, and Flask inside Python Workers. You can even create a Python Worker inside another Worker using [Dynamic Workers](https://blog.cloudflare.com/dynamic-workers/).

```
from fastapi import FastAPI, Request
from workers import asgi, WorkerEntrypoint

app = FastAPI()

@app.get("/")
async def root(request: Request):
    env = request.scope["env"]
    return await env.AI.run(
        "@cf/openai/gpt-oss-120b",
        {
            "instructions": "You are a friendly assistant.",
            "input": "What is the origin of the phrase Hello, World?",
        },
    )

Default = asgi.entrypoint(app)
```

## The journey behind Python Workers

Bringing Python to Cloudflare Workers was a natural choice. Because Workers has supported [WebAssembly since 2018](https://blog.cloudflare.com/webassembly-on-cloudflare-workers/), it gave us the perfect environment to run a Wasm-compiled Python interpreter. By using [Pyodide](https://pyodide.org/en/stable/), we were able to quickly support a wide range of Python applications in Cloudflare Workers.

Our goal was to create the first platform for infinitely scalable Python apps, while making it as easy and performant as developing Python apps anywhere else.

The features we are highlighting today are the result of this multi-year effort. Many developers are already building applications within Python Workers; today, we are making these capabilities production-ready for everyone.

## Python is now a first-class language in the Cloudflare Workers runtime

Python Workers now natively support Cloudflare Developer Platform bindings. Previously, using these Cloudflare bindings in Python Workers required converting Python objects into TypeScript objects explicitly at the RPC boundary. For example, sending a Python dictionary into a Cloudflare Queue required the following glue code to work:

```
from pyodide.ffi import to_js
import js

self.env.QUEUE.send(to_js({"key": "value"}, dict_converter=js.Object.fromEntries))
```

This required Python developers to keep the JavaScript environment and code in mind while writing Python Workers, and it was a common source of error for both humans and AI agents. To address this, we have [encapsulated the entire type conversion process](https://blog.cloudflare.com/python-workers-rpc/) within the Workers runtime and the Python SDK. This allows you to utilize all Cloudflare bindings in a Pythonic way without writing a single line of JavaScript code, making the following just work:

```
self.env.QUEUE.send({"key": "value"})
```

## Web frameworks: FastAPI, Django, and Flask

You can now run your favorite Python framework, such as FastAPI, Django, or Flask, to build an API server in Python Workers. We implemented a built-in connector that you can use to easily connect your web application to Python Workers.

Let’s say you have a simple FastAPI web application:

```
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    message = "Hello, world!"
    return {"message": message}
```

In native environments, you would use a web server such as `uvicorn` to run this application.

```
$ uvicorn main:app
```

In Python Workers, you can run the same application using the workers.asgi package we provide, just by adding this snippet to your code:

```
from workers import asgi

class Default(WorkerEntrypoint):
    async def fetch(self, request):
        return await asgi.fetch(app, request, self.env)

# or equivalently
Default = asgi.entrypoint(app)
```

Similarly, you can use `workers.wsgi` package to run synchronous web applications such as Django.

```
from workers import WorkerEntrypoint, wsgi
from your_django_app.wsgi import app

Default = wsgi.entrypoint(app)
```

### So, what happens under the hood?

Python has a standard contract for how web applications should communicate with web servers, known as the Web Server Gateway Interface (WSGI), or its modern asynchronous counterpart, ASGI. This standard allows developers to build applications that are completely server-agnostic. In a traditional deployment, web servers like Uvicorn or Gunicorn are responsible for handling multiple concurrent client connections and threads to scale traffic, while web frameworks like FastAPI can focus purely on the application logic.

In Cloudflare Workers, the Workers platform itself serves as the web server. Since our global network already seamlessly handles load balancing and infinite scaling, we don't need to reinvent the wheel by running a server inside Python Workers.

Instead, our `workers.asgi` and `workers.wsgi` connectors act as a thin, optimized bridge. They translate the incoming native JavaScript request into the standard WSGI/ASGI structures that Python applications expect, and seamlessly pipe the response back out with minimal overhead. By doing this, Python developers get the best of both worlds: you can write and organize code using your favorite web frameworks, while letting the Cloudflare Workers platform instantly scale your API across the globe, without ever configuring a server.

These connectors can be used not only with FastAPI, Django, or Flask, but with any Python web framework that uses the [WSGI](https://peps.python.org/pep-3333/) or [ASGI](https://asgi.readthedocs.io/en/latest/) interface.

You can find more information about using each web framework in the [Python Workers documentation](https://developers.cloudflare.com/workers/languages/python/packages/).

## Using PostgreSQL and MySQL with Hyperdrive

If you are building a Python application using relational databases such as PostgreSQL or MySQL, you can now integrate [Hyperdrive](https://developers.cloudflare.com/hyperdrive/) into Python Workers.

Previously, Python Workers didn’t support TCP sockets, making database drivers unavailable. To understand why this was a blocker, you need to look at how WebAssembly operates. Python database drivers like `aiomysql` or `asyncpg` rely on the standard library's `socket` module to establish connections. In a standard environment, this module makes POSIX system calls to the underlying operating system. Inside a WebAssembly sandbox, those POSIX networking syscalls are normally stubs that always fail. Any attempt to open a standard socket would immediately fail. To solve this problem, we implemented socket system calls using the Workers [connect](https://developers.cloudflare.com/workers/runtime-apis/tcp-sockets/) API.

When a database driver attempts to open a TCP connection, it goes through our custom socket syscall implementation. It translates standard Python socket operations like opening a connection and reading bytes into the corresponding JavaScript calls used by the Workers runtime. Because this translation happens at the system call level, your database drivers don't have to know about the underlying implementation at all.

This socket bridge is what makes our Hyperdrive integration possible. To use Hyperdrive in Python Workers, first connect your database with Hyperdrive and set up the binding in the Wrangler config:

```
"hyperdrive": [
    {
        "binding": "HYPERDRIVE_MYSQL",
        "id": "<example id: 57b7076f58be42419276f058a8968187>",
    }
]
```

Then, connect to Hyperdrive using the database drivers you are familiar with:

```
import aiomysql

from workers import WorkerEntrypoint

class Default(WorkerEntrypoint):
    async def fetch(self, request):
        hd = self.env.HYPERDRIVE_MYSQL
        conn = await aiomysql.connect(
            host=hd.host,
            port=int(hd.port),
            user=hd.user,
            password=hd.password,
            db=hd.database,
            ssl=None,
        )

        cur = await conn.cursor()
        await cur.execute("SELECT username FROM user")
        r = await cur.fetchall()
        await cur.close()
        conn.close()
```

You can refer to the [Hyperdrive Python Workers documentation](https://developers.cloudflare.com/hyperdrive/examples/python-workers/) to find out how you can use Hyperdrive in Python Workers, and which packages are currently supported.

## Expanding the WebAssembly package ecosystem

Because Python Workers run inside a WebAssembly sandbox, any packages with native C/C++/Rust extensions must be cross-compiled to WebAssembly to run in Python Workers. However, previously, there was no standard way to cross-compile any Python packages to WebAssembly. That meant our team had to manually compile and host custom WebAssembly packages. This greatly limited the number of packages you could actually use in Python Workers.

We wanted to fix this and allow users to use a wider variety of packages. However, we didn’t want to merely build packages usable only in Python Workers, which wouldn’t benefit the community. Since Python Workers are built on top of Pyodide, we wanted the ecosystem to evolve in a way that benefits Pyodide and the entire Python-on-WebAssembly community.

To this end, we proposed [PEP 783](https://peps.python.org/pep-0783/), which standardizes a platform for running Python in the browser runtimes called PyEmscripten. After over a year of discussion and refinement, this proposal was accepted, enabling package maintainers to build and publish packages for the PyEmscripten platform and make them available across all environments that implement PyEmscripten.

We also stabilized the existing Pyodide build toolchain and evolved it into a form that is accessible to all package maintainers, enabling developers to easily build packages for the PyEmscripten platform. Furthermore, we added PyEmscripten platform support to [`cibuildwheel`](https://cibuildwheel.pypa.io/en/stable/), to make it easier for others to adopt support for the PyEmscripten platform.

While the ecosystem is still adopting this standard, we hope every Python package will have a wheel that works with WebAssembly in the future. We are also actively working with major package maintainers to add PyEmscripten builds. If you encounter a package that isn’t supported yet, let us know on Discord or GitHub, and our team will work to get it built.

You can also check out our [EuroPython 2026 talk: “Python Everywhere: The State of Python on WebAssembly”](https://youtu.be/HklunTc9giA?si=wqeOjDl9qNe6qtB2) to see how we made this possible.

## Building AI agents and pipelines in Python

The large ecosystem of data science and machine learning packages makes Python the natural choice for building intelligent agents and AI pipelines. But bringing these to Python Workers historically presented a challenge: libraries such as `openai` and `langchain` rely on HTTP clients like `requests` or `httpx` to communicate with external APIs. However, because of missing low-level socket operations support in Python Workers, these HTTP clients didn’t work properly.

To solve this, we contributed upstream to ensure these HTTP clients can route requests directly through the JavaScript `fetch` API in WebAssembly environments. Combined with our new support for low-level socket operations as explained in [the previous section](https://blog.cloudflare.com/python-workers-ga/#using-postgresql-and-mysql-with-hyperdrive), this makes the entire networking stack work seamlessly inside Python Workers.

As a result, you can now run AI libraries like `openai`, `langchain`, and `mcp` natively in Python Workers. You can also combine them with Workers AI to run serverless inference on GPUs in Cloudflare’s network, or proxy requests through Cloudflare AI Gateway.

The example below shows a way to run Worker AI models in langchain, using the [langchain-cloudflare](https://github.com/cloudflare/langchain-cloudflare) package:

```
from langchain_cloudflare import ChatCloudflareWorkersAI
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import PromptTemplate
from workers import Response, WorkerEntrypoint

class Default(WorkerEntrypoint):
    async def fetch(self, request):
        prompt = PromptTemplate.from_template(
            "In one sentence, describe a great day in the life of an {profession}."
        )
        llm = ChatCloudflareWorkersAI(
            model_name="@cf/meta/llama-3.3-70b-instruct-fp8-fast",
            binding=self.env.AI,
            max_tokens=64,
        )
        chain = prompt | llm | StrOutputParser()

        result = await chain.ainvoke({"profession": "electrician"})
        return Response.json({"result": result})
```

## What you can build today

We have assembled a collection of production-ready patterns in our [python-workers-examples](https://github.com/cloudflare/python-workers-examples) repository. Here are some ways you can combine Python Workers with the Cloudflare ecosystem.

### Asynchronous AI orchestration

Building a full-stack AI application often means connecting multiple services such as storage, queuing, and inference. [This example](https://github.com/cloudflare/python-workers-examples/tree/main/image-redraw) shows how to build an AI-driven image-to-image generator purely in Python Workers. It accepts user requests, drops them into a Cloudflare Queue, and uses Workflows to orchestrate the image generation step via Workers AI, and stores the image to an R2 bucket.

![BLOG-3512 2.png](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M2V34B98AFHX986KP70WF54Y.01M2V34CGNFYXX59FZ2MPKEKWK.png&w=1430&h=650&f=webp&fit=cover&position=center)

### Real-time stream processing with Bluesky Jetstream

Consuming a firehose of real-time events usually requires a dedicated server to maintain the connection. In this example, we use [a Python Worker to connect to the ATProto/Bluesky Jetstream WebSocket](https://github.com/cloudflare/python-workers-examples/tree/main/websocket-stream-consumer). By backing this connection with a Durable Object, the Python Worker can maintain long-lived state, ensuring that the WebSocket connection stays alive.

![BLOG-3512 3.png](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M2V34BJ3JC9ESH8HE7K14QS3.01M2V34CN9240HX0Y61C6R57TE.png&w=1430&h=276&f=webp&fit=cover&position=center)

### More examples to explore

#### Model Context Protocol (MCP) Server

[Build and deploy an MCP server](https://github.com/cloudflare/python-workers-examples/tree/main/mcp-server) using the official Python MCP package to give your AI assistants access to edge data.

![BLOG-3512 4.png](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M2V34F3DBEWD2N6YV1ZS9XV1.01M2V34G4DP7AAFRGZTS13W1EF.png&w=1430&h=276&f=webp&fit=cover&position=center)

#### Retrieval-Augmented Generation (RAG) system with Vectorize

[Building a RAG system](https://github.com/cloudflare/python-workers-examples/tree/main/vectorize-rag) using Workers AI and Vectorize, Cloudflare’s vector database.

![BLOG-3512 5.png](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M2V34FR0NPET1PKA1YGE6QS7.01M2V34GSM4JXGX4DMA1H393CS.png&w=1430&h=728&f=webp&fit=cover&position=center)

## Python code examples across the Cloudflare developer docs

We’ve updated our docs across Cloudflare products to include Python example code. Nearly everywhere where there is a code example showing how to do something in TypeScript, there’s also a code example in Python. We’re committed to continuing to include Python examples across all of our products. You can toggle code snippets between JavaScript, TypeScript, and Python throughout our developer documentation.

![BLOG-3512 6.png](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M2V34GW5EXQCQW27RV0EWYEH.01M2V34J33FCM8TBEM9TXQ098T.png&w=1430&h=1028&f=webp&fit=cover&position=center)

## What’s next?

Reaching GA is just the start. We have many plans to make Python Workers better, including making Python Workers more performant and memory efficient, as well as supporting more packages.

Keep telling us what you want to build on Python Workers, and we’ll keep pushing the bounds of what is possible. Check out [Python Workers documentation](https://developers.cloudflare.com/workers/languages/python/) and start building your first Python Worker!
