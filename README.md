# Cloudflare Error Pages

Cloudflare-Error is a set of clean and professional HTML templates designed for use as error pages in Cloudflare's [custom page](https://support.cloudflare.com/hc/en-us/articles/200172706-Configuring-Custom-Pages-Error-and-Challenge-) section. This repository uses the latest version of Tailwind CSS with tree shaking and an optimized build system.

## Development

This repository now includes a modern build system with Tailwind CSS tree shaking:

```bash
# Install dependencies
npm install

# Build CSS (development)
npm run build

# Build CSS (production, minified)
npm run build:prod

# Watch for changes during development
npm run dev
```

## Installation

Modify each template to your liking and use a service such as Cloudflare Pages to host your HTML templates temporarily. Then, head over to your custom page settings in your organisation's Cloudflare account and add each page as required using the full URL.

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## Acknowledgements

-   [Refactoring UI](https://refactoringui.com/)
-   [Tailwind UI](https://tailwindui.com/)

## License

[MIT](https://choosealicense.com/licenses/mit/)

<!--
    ---
    title: Error page types · Cloudflare Rules docs
    description: "The following error page types will only be shown in the
    Cloudflare dashboard if you have customized their error pages in the past:"
    lastUpdated: 2025-06-04T08:56:32.000Z
    chatbotDeprioritize: false
    source_url:
    html: https://developers.cloudflare.com/rules/custom-errors/reference/error-page-types/
    md: https://developers.cloudflare.com/rules/custom-errors/reference/error-page-types/index.md
    ---
-->

## Error page types

| Page type                                 | Description                                                                                                                                                                                                                                                                                                         | API identifier      |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| WAF block                                 | The page displayed when visitors are blocked by a [Web Application Firewall](https://developers.cloudflare.com/waf/) rule. This page returns a `403` status code.                                                                                                                                                   | `waf_block`         |
| IP/Country block                          | The page displayed when a request originates from a [blocked IP address or country](https://developers.cloudflare.com/waf/tools/ip-access-rules/). This page returns a `403` status code.                                                                                                                           | `ip_block`          |
| IP/Country challenge                      | Presents a challenge to visitors from specified IP addresses or countries. This page returns a `403` status code. For more information, refer to [IP Access rules](https://developers.cloudflare.com/waf/tools/ip-access-rules/).                                                                                   | `country_challenge` |
| 500 class errors                          | 500 class error pages are displayed when a web server is unable to process a request. For more information, refer to [Cloudflare 5XX errors](https://developers.cloudflare.com/support/troubleshooting/http-status-codes/cloudflare-5xx-errors/).                                                                   | `500_errors`        |
| 1000 class errors                         | 1000 class error pages are displayed when a domain’s configuration, security settings, or origin setup prevents Cloudflare from completing a request. For more information, refer to [Cloudflare 1XXX errors](https://developers.cloudflare.com/support/troubleshooting/http-status-codes/cloudflare-1xxx-errors/). | `1000_errors`       |
| Managed challenge / I'm Under Attack Mode | Presents different types of challenges to a visitor depending on the nature of their request and your security settings. This page returns a `403` status code. For more information, refer to [Under Attack mode](https://developers.cloudflare.com/fundamentals/reference/under-attack-mode/).                    | `managed_challenge` |
| Rate limiting block                       | Displayed to visitors when they have been blocked by a [rate limiting rule](https://developers.cloudflare.com/waf/rate-limiting-rules/). This page returns a `429` status code.                                                                                                                                     | `ratelimit_block`   |

The following error page types will only be shown in the Cloudflare dashboard if you have customized their error pages in the past:

| Page type             | API identifier    |
| --------------------- | ----------------- |
| Interactive Challenge | `basic_challenge` |
| JavaScript Challenge  | `under_attack`    |

These types of challenges are being discouraged in favor of managed challenges. Refer to [Challenge pages](https://developers.cloudflare.com/cloudflare-challenges/challenge-types/challenge-pages/) for more information.

<!--
    ---
    title: Custom error tokens · Cloudflare Rules docs
    description: Each custom error token provides diagnostic information or specific
    functionality that appears on the error page. Certain error pages require a
    page-specific custom error token.
    lastUpdated: 2025-08-11T18:12:25.000Z
    chatbotDeprioritize: false
    source_url:
    html: https://developers.cloudflare.com/rules/custom-errors/reference/error-tokens/
    md: https://developers.cloudflare.com/rules/custom-errors/reference/error-tokens/index.md
    ---
-->

## Error tokens

### For Error Pages

Each custom error token provides diagnostic information or specific functionality that appears on the error page. Certain error pages require a page-specific custom error token.

To display a custom page for each error, create a separate page per error. For example, to create an error page for both **IP/Country Block** and **Interactive Challenge**, you must design and publish two separate pages.

The following custom error tokens are required by their respective error pages:

| Token                            | Required for                                                                                                              |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `::CAPTCHA_BOX::`                | Interactive Challenge Country Challenge (Managed Challenge) Managed Challenge / I'm Under Attack Mode (Interstitial Page) |
| `::IM_UNDER_ATTACK_BOX::`        | Non-Interactive Challenge (JS Challenge)                                                                                  |
| `::CLOUDFLARE_ERROR_500S_BOX::`  | 5XX Errors                                                                                                                |
| `::CLOUDFLARE_ERROR_1000S_BOX::` | 1XXX Errors                                                                                                               |

Each custom error token has a default look and feel. However, you can use CSS to stylize each custom error tag using each tag's class ID. All the external resources like images, CSS, and scripts will be inlined during the process. As such, all external resources need to be available (that is, they must return `200 OK`) otherwise an error will be thrown.

### For Custom Error Assets, inline responses, and Error Pages

A custom error asset, inline response, or error page may also include the following error tokens, which will be replaced with their real values before sending the response to the visitor:

| Token           | Description                                                              |
| --------------- | ------------------------------------------------------------------------ |
| `::CLIENT_IP::` | The visitor's IP address.                                                |
| `::RAY_ID::`    | A unique identifier given to every request that goes through Cloudflare. |
| `::GEO::`       | The country or region associated with the visitor's IP address.          |
