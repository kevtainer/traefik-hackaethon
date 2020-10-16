
# Traefik Hackaethon 2020: Middleware Plugins Brain Dump

<img src="https://containous.ghost.io/content/images/2020/10/Traefik-Hackaethon-2020---Middleware-Plugins-Brain-Dump.jpg">
One of the most powerful features of the Traefik software-based load balancer is its support for <a href="https://doc.traefik.io/traefik/middlewares/overview/">middlewares</a>. These packages are responsible for applying transformations, validations, redirections, omissions, and additions to requests before passing them on, either to another middleware package or to their final destination.

Over the past several years, Traefik has received dozens of ideas and concepts for middlewares through feature and pull requests. Some of these proposals suggested additional functionality for existing middlewares. Others introduced bespoke middleware for specific use cases.

When these proposals weren't aligned to the vision of the Traefik community they languished on the Traefik issue board. There they lay dormant, waiting for the day that users could easily build <a href="https://traefik.io/blog/unleash-custom-networking-logic-with-traefik-plugins/">custom plugins for Traefik</a>. That day is here, and this post will provide an outline for those interested in building and extending the capabilities of Traefik with their own middlewares.

Here are some great ideas for middleware plugins that drew from issues on the Traefik repository.

## Popular Middleware Ideas

### Query Parameter Modification

<a href="https://github.com/containous/traefik/issues/6276">https://github.com/containous/traefik/issues/6276</a>

In some cases, operators may want to modify the request's query keys or convert query parameters into a path. In either scenario, this can be useful for several use cases. But along with the usefulness comes additional complexity in test scenarios and implementations. These constraints make it a prime candidate for adoption as a custom plugin.

### Header Transformation

<a href="https://github.com/traefik/traefik/issues/6047">https://github.com/traefik/traefik/issues/6047</a>

The scope of the Header middleware's current implementation is limited to adding or removing headers (which is only available through a file type provider). Users may want to perform more complex operations on headers, such as renaming a key or transforming a value. These operations could be achievable through regex or simple pattern matching.

### Enhanced Retry and Backoff

<a href="https://github.com/traefik/traefik/issues/5282">https://github.com/traefik/traefik/issues/5282</a>
<a href="https://github.com/traefik/traefik/issues/4578">https://github.com/traefik/traefik/issues/4578</a>

Cases exist where operators may want to retry on certain conditions, such as a 5xx error or connection refused. Without exponential backoff or retries based on response code, retrying on those parameters isn't feasible. For additional inspiration, <a href="https://cloud.spring.io/spring-cloud-gateway/reference/html/#the-retry-gatewayfilter-factory" target="_blank" rel="nofollow">Spring Cloud Gateway</a> has a design that handles both cases.

### Custom Response Code Overrides

<a href="https://github.com/traefik/traefik/issues/2039">https://github.com/traefik/traefik/issues/2039</a>

When users receive an error message, this can sometimes indicate a particular resource's presence or absence. Also, when it’s impossible to modify the client’s behavior this can be worked-around by intercepting an erroneous response and handling it by return a different error response or page. This plugin would effectively intercept the service's error response and map an alternative response, such as a 200 response instead of a 501 response, or a 404 rather than a 401 or 403.

### CDN AllowList

<a href="https://github.com/traefik/traefik/issues/4145">https://github.com/traefik/traefik/issues/4145</a>

CDNs publish their IP addresses from consumable public resources. When hosting a particular resource that should only be accessible from the CDN (for cache-fronting reasons), it’s logical that users want to block any request that does not originate from an IP address published on that list.

### BasicAuth Overrides

<a href="https://github.com/traefik/traefik/issues/4429">https://github.com/traefik/traefik/issues/4429</a>

In some instances, users have asked to allow Basic Auth to allow programmatic overrides by either a custom header or originating from a specific IP address (or set of IP addresses). Both options could be implemented into a custom authentication plugin, with support for IP ranges as well.

### Rate Limiting Enhancements

<a href="https://github.com/traefik/traefik/issues/4548">https://github.com/traefik/traefik/issues/4548</a>
<a href="https://github.com/traefik/traefik/issues/6042">https://github.com/traefik/traefik/issues/6042</a>

Users have requested the ability to incorporate multiple rate limiting strategies concurrently; for example, combining client.ip and request.host on a single frontend. Users have also asked to store rate limiting data external to Traefik, enabling more extended duration limits to survive restarts and share limits across multiple instances. These would likely be two separate plugins as they’re two distinctly different features.

### Response Header Redirect

<a href="https://github.com/traefik/traefik/issues/5154">https://github.com/traefik/traefik/issues/5154</a>

There is a request to allow a service to command Traefik to internally redirect to another service provider through a custom header. This capability could be useful when the client doesn't require exposure to an internal redirect's details.

### Brotli Compression

<a href="https://github.com/traefik/traefik/issues/4202">https://github.com/traefik/traefik/issues/4202</a>

This plugin has already been implemented as a PR to Traefik. However, the licensing model on one of the dependencies prevented it from being merged as-is. This compression middleware would be relatively straightforward to implement as a plugin.

### Additional Plugin Ideas

There are other plugin ideas out there. We don't want to limit your creativity, either, so plugin bounties are not limited to this list. As long as the plugin isn't trivial and is deemed functionality by the judges, it is eligible for the prize. Here are a few more issues if you're not interested in the ideas listed above.

- HTTP Cache Backends - <a href="https://github.com/traefik/traefik/issues/878">https://github.com/traefik/traefik/issues/878</a>
- Containers on Demand - <a href="https://github.com/traefik/traefik/issues/6993">https://github.com/traefik/traefik/issues/6993</a>
- Open Policy Agent - <a href="https://github.com/traefik/traefik/issues/4894">https://github.com/traefik/traefik/issues/4894</a>

> src: [https://traefik.io/blog/traefik-hackaethon-2020-middleware-plugins-brain-dump/](https://traefik.io/blog/traefik-hackaethon-2020-middleware-plugins-brain-dump/)
