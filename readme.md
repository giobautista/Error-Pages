# Error Pages

By default the error messages in nginx are hardcoded and are not really nice looking. It is possible to show your own custom error pages. In order to so, a minor change to the config file is needed.

You can either use the default nginx config (`/etc/nginx/nginx.conf`) or the config of a single site (`/etc/nginx/sites-enabled/example.com`). I prefer the latter.

Add the following lines to the server block:

```
# Custom error pages
error_page 401 /error/401/index.html;
error_page 403 /error/403/index.html;
error_page 404 /error/404/index.html;
error_page 405 /error/405/index.html;
error_page 500 501 502 503 504 /error/5xx/index.html;
location ^~ /error/ {
  internal;
}
```

You only need to add the definitions for the HTTP status codes you want to support. If you don’t need a custom error page for 401 errors, just leave it out from the config file and nginx will serve the default response.

In the example above, all error pages will be served from the directory error within the document root of the current site. If you want to serve the files from a common directory for all configured site then you need something like this:

```
location ^~ /error/ {
  internal;
  root /path/to/common/error-files;
}
```

In both cases you’ll notice the line that says internal. This prevents visitors to fetch the error pages directly.

Save the files and reload the nginx config. Now your own custom error pages will be used!
