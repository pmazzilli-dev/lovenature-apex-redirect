# lovenature-apex-redirect

Redirige el **dominio raíz** `lovenaturejkremoval.com` (sin `www`) hacia el sitio real
**`https://www.lovenaturejkremoval.com`**.

## Por qué existe

El sitio de Love Nature Junk Removal está hospedado en **Cloudflare Pages** y se sirve en
`www.lovenaturejkremoval.com` (vía un CNAME). Pero el **DNS del dominio está en Wix**, y:

- Wix **no permite cambiar los nameservers** (así que no se puede usar el DNS de Cloudflare), y
- el estándar DNS **no permite un CNAME en la raíz** de un dominio.

Resultado: el dominio "pelado" (`lovenaturejkremoval.com`) no podía apuntar a Cloudflare Pages y daba
error 404.

**GitHub Pages sí publica IPs fijas para dominios raíz**, así que este repo se usa como un
redireccionador liviano: la raíz apunta (registros `A`) a GitHub Pages, que sirve este contenido y
reenvía al `www`.

## Cómo funciona

- [`index.html`](index.html) y [`404.html`](404.html) redirigen a `https://www.lovenaturejkremoval.com`
  preservando ruta/query/hash (JS + `<meta refresh>` de respaldo). El `404.html` captura cualquier ruta.
- [`CNAME`](CNAME) fija el dominio personalizado (`lovenaturejkremoval.com`) en GitHub Pages.

## Configuración DNS (en Wix)

Registros **A** de la raíz apuntando a las IPs de GitHub Pages:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

El `www` **no** se toca (sigue en Cloudflare Pages). GitHub Pages emite el certificado HTTPS del
dominio raíz automáticamente.

> Solución temporal/pragmática mientras el dominio siga registrado en Wix. La opción "definitiva"
> sería transferir el dominio a un registrador que permita usar los nameservers de Cloudflare.
