---
order: 1
pcx-content-type: how-to
---

# Manage subdomains

Once you have [added your domain to Cloudflare](https://support.cloudflare.com/hc/articles/201720164#2YulMb5YJTVnMxgAgNWdS2) and [updated your nameservers](https://support.cloudflare.com/hc/articles/205195708), you also might want to set up a subdomain (`www.example.com` or `blog.example.com`).

## Create a subdomain

Most subdomains serve a specific purpose within the overall context of your website. For example, `blog.example.com` might be your blog, `support.example.com` could be your customer help portal, and `store.example.com` would be your e-commerce site.

If you have already added that subdomain at your host, create a corresponding [DNS A or CNAME record](https://support.cloudflare.com/hc/articles/360019093151) for that subdomain (`blog`, `store`).

## Set up redirects

### Redirect a subdomain to root domain

Sometimes, you might want to create a subdomain (`www.example.com`) that simply redirects traffic to your root domain (`example.com`).

1. Create a [proxied DNS A record](https://support.cloudflare.com/hc/articles/360019093151) for your subdomain. This record can point to any IP address since all traffic will be redirected prior to reaching the address.

    <Example>

    | **Type** | **Name** | **IPv4 address** | **Proxy status** |
    | --- | --- | --- | --- |
    | A | `www` | `192.0.2.1`| Proxied |

    </Example>

1. Use [Bulk redirects](https://developers.cloudflare.com/rules/bulk-redirects) to forward traffic from your subdomain to your root domain. You will likely want to include **Subpath matching** to ensure requests to `www.example.com/examples` go to `example.com/examples`.

    <Example>
    
    | **Source URL** | **Target URL** | **Status** | **Selected parameters** |
    | --------- | --------- | --- | --- |
    | `https://www.example.com` | `https://example.com` | 301 | *Subpath matching* and *Preserve path suffix* |
    
    </Example>

### Redirect root domain to a subdomain

Sometimes, you might want all traffic to your root domain (`example.com`) to actually go to a subdomain (`www.example.com`).

1. If you have already added that subdomain at your host, create a corresponding [DNS A or CNAME record](https://support.cloudflare.com/hc/articles/360019093151) for that subdomain.
1. Create a proxied DNS A record for your root domain. This record can point to any IP address since all traffic will be redirected prior to reaching the address.

    <Example>

    | **Type** | **Name** | **IPv4 address** | **Proxy status** |
    | --- | --- | --- | --- |
    | A | `@` | `192.0.2.1`| Proxied |

    </Example>

1. Use [Bulk redirects](https://developers.cloudflare.com/rules/bulk-redirects) to forward traffic from your root domain to your subdomain. You will likely want to include **Subpath matching** to ensure requests to `example.com/examples` go to `www.example.com/examples`.

    <Example>
    
    | **Source URL** | **Target URL** | **Status** | **Selected parameters** |
    | --------- | --------- | --- | --- |
    | `https://example.com` | `https://www.example.com` | 301 | *Subpath matching* and *Preserve path suffix* |
    
    </Example>

## SSL/TLS for subdomains

If your main domain is using Cloudflare's [Universal SSL certificate](https://developers.cloudflare.com/ssl/edge-certificates/universal-ssl), that certificate also covers all first-level subdomains (`blog.example.com`). 

For deeper subdomains (`dev.blog.example.com`), use a [different type of certificate](https://developers.cloudflare.com/ssl/edge-certificates/universal-ssl/limitations#full-setup).

## Customize Cloudflare settings per subdomain

To learn more about customizing Cloudflare settings for individual subdomains, refer to [Subdomain support](https://support.cloudflare.com/hc/articles/360026440252).