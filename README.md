# Clifford Training & Coaching — Site referentie

Dit ZIP-bestand bevat een statische HTML-referentie voor de nieuwe site van cliffordtraining.nl. Het is bedoeld als uitgangspunt voor de WordPress/Kadence-implementatie. Teksten, foto's, structuur, styling en SEO-meta zijn allemaal klaar voor overname.

## Structuur

```
├── index.html                                 Homepage
├── over-dave.html                             Over Dave (volledig CV)
├── contact.html                               Contactpagina
├── aanbevelingen.html                         15 uitgelichte aanbevelingen
├── privacy.html, cookies.html, voorwaarden.html   Juridisch
├── sitemap.xml, robots.txt                    SEO-bestanden
├── coaching/
│   ├── executive-coaching.html
│   ├── loopbaancoaching.html
│   └── burnout-coaching.html
├── training/
│   ├── praktisch-leidinggeven.html
│   ├── verandermanagement.html
│   └── persoonlijk-leiderschap.html
├── blog/
│   ├── index.html
│   ├── pijn-van-discipline-versus-pijn-van-spijt.html
│   ├── zelfstandigheid-van-je-team-vergroten.html
│   ├── effectief-stakeholder-management.html
│   └── conflicten-oplossen-met-sociaal-panorama.html
└── assets/
    ├── style.css
    ├── logo-clifford.svg                      Donkere variant (header)
    ├── logo-clifford-white.svg                Crème variant (footer op donkerblauw)
    └── img/                                   Geoptimaliseerde foto's (q82-q88)
```

## Kerngegevens

- **Naam**: Clifford Training & Coaching
- **Adres**: Eusebiusbuitensingel 7, 6828 HT Arnhem
- **Telefoon**: 06 42 24 90 79
- **E-mail**: info@cliffordtraining.nl
- **LinkedIn**: https://www.linkedin.com/in/cliffordtraining
- **KVK**: 09183617
- **BTW**: NL001970310B73

## Training- en coachingslocatie

Coaching: Arnhem of online, Nederlands én Engels.
Trainingen: bij klant op locatie, of voor verdiepingsprogramma's op **ROOTS Inspiratielocatie** in **Haaren, Noord-Brabant** (https://www.roots.nl/).

## Design tokens

```css
--ink: #0E1B2C            /* donkerblauwe tekst */
--ink-soft: #3D4A5C       /* zachter grijsblauw */
--paper: #F5F1EA          /* crème achtergrond */
--paper-warm: #EDE7DC     /* iets warmere crème */
--accent: #C25F3C         /* terracotta accent */
--line: #D9D2C4           /* fijne lijnen */
```

**Fonts**: Fraunces (serif, voor titels en quotes) + Inter (sans-serif, body). Beide via Google Fonts, preconnect is al ingesteld.

## SEO & performance

Elke pagina bevat:
- `<html lang="nl">`, viewport, canonical, charset
- Meta description (gericht op zoekgedrag van senior leiders)
- Open Graph + Twitter Card tags
- `theme-color` (#0E1B2C) voor mobiele browsers
- Favicon als SVG
- `rel="preconnect"` voor Google Fonts
- `loading="eager"` en `fetchpriority="high"` voor hero-afbeelding
- `loading="lazy"` voor overige afbeeldingen
- `width` en `height` op álle `<img>` tags (voorkomt layout shift)
- Structured data (JSON-LD) voor ProfessionalService, Person, WebSite, Service, BlogPosting, Review, LocalBusiness

## Copy-stijl

Teksten zijn geschreven in **FD-stijl** (scherp, zakelijk, Nederlands dagblad):
- Géén em-dashes (—) of hyphens met spaties als gedachtestreep
- Wél komma, puntkomma, dubbele punt of haakjes
- Non-breaking hyphen (`&#8209;`) in samengestelde termen als `NLP‑masteropleiding`
- Typografische aanhalingstekens: `&ldquo;` en `&rdquo;`
- Auteur-attributie in blockquote met komma: `<strong>Naam</strong>, Rol, Bedrijf`

## Security-headers voor de Fiverr-bouwer

Zet deze in `.htaccess` (Apache/TransIP) of in de Nginx-config. Ze maken de site veiliger en SEO-proofer:

```apache
# Security headers
Header always set X-Content-Type-Options "nosniff"
Header always set X-Frame-Options "SAMEORIGIN"
Header always set Referrer-Policy "strict-origin-when-cross-origin"
Header always set Permissions-Policy "geolocation=(), microphone=(), camera=()"
Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"

# CSP (Content Security Policy)
Header always set Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline' https://www.googletagmanager.com https://plausible.io; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src 'self' https://fonts.gstatic.com; img-src 'self' data: https:; connect-src 'self' https://plausible.io; frame-ancestors 'self';"

# Redirect HTTP naar HTTPS
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]

# Redirect non-www naar www
RewriteCond %{HTTP_HOST} !^www\. [NC]
RewriteRule ^ https://www.%{HTTP_HOST}%{REQUEST_URI} [L,R=301]

# Gzip compression
<IfModule mod_deflate.c>
  AddOutputFilterByType DEFLATE text/html text/css text/javascript application/javascript application/json image/svg+xml
</IfModule>

# Browser caching
<IfModule mod_expires.c>
  ExpiresActive On
  ExpiresByType image/jpg "access plus 1 year"
  ExpiresByType image/jpeg "access plus 1 year"
  ExpiresByType image/svg+xml "access plus 1 year"
  ExpiresByType image/webp "access plus 1 year"
  ExpiresByType text/css "access plus 1 month"
  ExpiresByType text/html "access plus 1 week"
</IfModule>
```

## WordPress-migratieaandachtspunten

1. **Permalinks**: `/blog/%postname%/` voor blogartikelen
2. **Featured images**: elke blog heeft een hero-foto, gebruik deze als featured image
3. **Categorieën**: Leiderschap, Management, Stakeholdermanagement, Sociaal Panorama
4. **Schema-markup**: Rank Math of Yoast SEO Pro voor automatische structured data
5. **Logo in Kadence**: upload `logo-clifford.svg` als main logo, `logo-clifford-white.svg` als footer/dark logo
6. **Calendly**: placeholder `https://calendly.com/cliffordtraining` op contactpagina — vervangen door juiste link
7. **Cookie-banner**: Complianz of CookieYes bij WordPress-installatie
8. **Analytics**: Plausible aanbevolen (cookieloos)

## Lighthouse-score verwachting

Met de huidige optimalisaties:
- Performance: 95+ (geoptimaliseerde foto's, lazy loading, preconnect, minimaal CSS)
- Accessibility: 95+ (lang, alt-tags, focus states, aria-labels)
- Best Practices: 100 (HTTPS-headers aanbevolen in .htaccess)
- SEO: 100 (canonical, meta, structured data, sitemap, robots.txt)

## Let op bij overname

- De **contactpagina** bevat een placeholder Calendly-link
- De **aanbevelingen-pagina** toont 15 aanbevelingen; volledige 74 staan op LinkedIn
- De **trust-bar** op de homepage bevat namen van klantorganisaties als tekst; in WordPress eventueel vervangen door logo-slider
- De **hero-image** op de homepage is `01-portret-raam-staand.jpg`; object-position `center 30%` voor goede gezichtsframing

Vragen? Mail **info@cliffordtraining.nl** of bel 06 42 24 90 79.

---

*Laatste update: april 2026*

