
```c 
/// 초기화 단계에서의 사용시 모든 이벤트에 첨부 
sentry_options_add_attachment(options, "/var/server.log");
```

```c title:'scope에서의 사용'
// Global Scope
sentry_attach_file("/var/global.log");

// Local Scope
sentry_scope_t *scope = sentry_local_scope_new();
sentry_scope_attach_file(scope, "/var/local.log");
sentry_value_t event = sentry_value_new_event();
/* ... */
sentry_capture_event_with_scope(event, scope);
```


```c title:'byte 변수 활용 초기화 방법'
char *bytes = ...;
size_t bytes_len = 123;

// Global Scope
sentry_attach_bytes(bytes, bytes_len, "global.bin");

// Local Scope
sentry_scope_t *scope = sentry_local_scope_new();
sentry_scope_attach_bytes(scope, bytes, bytes_len, "local.bin");
sentry_value_t event = sentry_value_new_event();
/* ... */
sentry_capture_event_with_scope(event, scope);

```


글로벌 범위에서 첨부 파일을 제거하려면 sentry_attach_file에서 반환 한 sentry_attachment_t 핸들을 사용할 수 있습니다. 첨부 파일을 제거한 후 파일은 더 이상 향후 이벤트 나 충돌로 업로드되지 않으며 핸들이 유효하지 않습니다.

```c
sentry_attachment_t *attachment = sentry_attach_file("/var/temp.log");
/* ... */
sentry_remove_attachment(attachment);
```