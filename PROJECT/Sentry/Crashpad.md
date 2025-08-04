
```c title:'crashpad exe, path설정'
    sentry_options_t *options = sentry_options_new();
    sentry_options_set_handler_path(options, "path/to/crashpad_handler");
    sentry_options_set_database_path(options, "sentry-db-directory");
    sentry_init(options);
```