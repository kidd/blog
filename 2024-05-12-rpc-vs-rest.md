---
title: REST vs RPC
categories: [programming]
---
I think there are a few concepts that have been derailed from their
original meaning...

In the old world, RPC meant stuff like corba, which was a full blown
OOP framework for distributed computing I think. I think it sold full
discoverability of methods and "run things as if they were on client,
and we'll run it wherever it is, but you, as a client, don't have to
worry". Reality is it meant it neede full sync of client and server
codes. And knowing how shaky everything was in those Java 1.0
times. Imagine the complexity of Hibernate but for RPC, in 1998. Wow.
It had binary protocols for calls and return values, and the problems
of "well... I can't really pass a socket or a pointer through a rpc,
so it can't be reeaaaallly transparent".  So that was transport
protocol, serialization, discoverability... a lot of things.

Then there was SOAP wbservices (wdsl), which was a way of calling
remote functions, given by an xml spec that contained the specs of the
available functions, their parameters and their return values.  It was
already using HTTP in a single `POST /`, but the sessions were authed
in the beginning, and later you kinda got a "connected client", and
did operations there.  Internally, the wsdl protocol used only a
single post endpoint and funneled everything through body
parameters. It serialized and deserialized in client and servers'
code, but it was transparent to you. It also needed care in clients
when there were updates in server code/specs. At that time,
versionning was not very popular yet (in the domains I worked at
least).

Graphql uses also POST at `/` and they don't give any fucks about
mapping resources into paths. I see that as a case of "throw my hands
up in the air, declare bankrupcy and do my own shit from scratch, that
I know it works, because I'm shoehorning things into protocols and I
feel stupid when I spent too many hours on this while it should be
easy to do".

Go has gRPC, which is a thinner framework for rpc and serialization
using protobuffers, but I think it also requires code synchronization,
and code generation for the new endpoints.  K8s works this way
internally between the internal services of it, but when it offers an
external api, it gives a rest api, defined by yamls, that are
generated from the grpc specs [citation needed]. (hairball-gets-bigger).

The autodisoverability is the same as what OpenAPI provides, but
openapi is totally loosely coupled with the reality of the api. I can
publish an outdated openapi that doesn't match the real api.

So there's 2 things

* "transport", whether it's http or not (grpc uses different ports and
  protocols),
* and there's the fact that clients and servers talk "seamlessly"
  calling back and forth calls.

In a world that the communication is only rpc, in principle you
woudn't even know if a call happens locally or not. That's cool in
controlled envs, but still dangerous for delays, network issues,
etc...

I think making HTTP more explicit was a way towards "microservices",
and put a narrow waist inbetween programs. Imagine that your app
connects with a socket to an external server and it has no way talking
to the server unless you "know the language" of what to send through a
socket, and what to expect back.  I think due to network issues
concerns, the stateless thing and "retryiability" got more traction.

On REST apis, the focus is on the entities, and it kinda surfaces this
to the client. Making it more about data and nouns than verbs.  Idk if
this is good or bad, but even when we have those `PUT
/something/122/actions/do-this` (which is not super RESTy, but it's
what we have to do when REST is not just about simple CRUD), there's
an implicit `self` there, which is the `something` object with
`id=122`.

Nowadays public apis offer things on REST-ish endpoints. And for
"rpc-like" experience you have sdks, that simulate the rpc experience
for the developer. (when we use the stripe sdk, it's doing http calls
beneath, but it does the serialization and checks client <-> server )

On our particular case, I think we're a tiny grain of sand in the
desert of the industry, and what we do doesn't matter much, but I
don't feel the mission of recreating anything. I use the minimum
amount of features that make my thing work.  I use the minimum amount
of features that make my thing work. But my api is not really REST and
it's a bit more pragmatic IMHO. We sometimes aggregate many entities
in the same responses if frontend needs them together in the same
view.

- the path is a namespace and an "address" for that resource. hopefully stable on time.
- GET if it's a read and can be retried
- POST if it's an insert
- PUT everything else

I find new headers to me every other day (ETag?), and it seems headers
are used as a backchannel to pass things that "shouldn't be there, but
are needed, so :man-shrugging:" and the industry agrees that headers
is where people put their dirty metadata/shit.

Somehow it seems REST is focused on doing less, and gravitate to show
the data and actions being the "weird thing", and rpc is more based on
tight coupling and less restrictions. A bit of "worse is better", and
it would explain why REST won.

Bret Victor's talk about the real way to communicate
https://youtu.be/8pTEmbeENF4?si=eloQA5l7tvcwd5rD&t=772.

Rich Hickey and Alan Kay discussion on data vs
interpreters/ambassadors https://news.ycombinator.com/item?id=11945722

Dylan Beattle talk on REST: https://www.youtube.com/watch?v=g8E1B7rTZBI
