# Menjalankan-Channell-di-Django-1.11

- Sumber dari:
<a https://gearheart.io/blog/creating-a-chat-with-django-channels/ target="_blank">Multi Chat</a>
- Web Kami:
<a http://www.kspra.co.id target="_blank">Aplikasi Web</a>

1. Setelah Menjalankan VirtualEnv, jalan perintah:
```
$pip install -r tedi_install.txt
$django-admin.py startproject multichat
$cd multichat
$./manage.py migrate

$./manage.py createsuperuser --username admin --email tedi_black@ksura.co.id
```
2. Ubah Di multichat/setting.py

```
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles','channels',
]

redis_host = os.environ.get('REDIS_HOST', 'localhost')

# Channel layer definitions
# http://channels.readthedocs.org/en/latest/deploying.html#setting-up-a-channel-backend
CHANNEL_LAYERS = {
    "default": {
        # This example app uses the Redis channel layer implementation asgi_redis
        "BACKEND": "asgi_redis.RedisChannelLayer",
        "CONFIG": {
            "hosts": [(redis_host, 6379)],
        },
       "ROUTING": "multichat.routing.channel_routing", # We will create it in a moment
    },
}

```

3. Jalankan shell python,ketikan menjalankan runserver harun di jalankna ,$./manage.py  shell

```
>>> import websocket
>>> ws = websocket.WebSocket()
>>> ws.connect("ws://localhost:8000")

>>> ws.send('Hello')
>>> 18
###Harus keluar seperti ini
Django version 1.11.2, using settings 'multichat.settings'
Starting Channels development server at http://127.0.0.1:8000/
Channel layer default (asgi_redis.core.RedisChannelLayer)
Quit the server with CONTROL-C.
2019-05-10 17:41:46,828 - INFO - worker - Listening on channels http.request, websocket.connect, websocket.disconnect, websocket.receive
2019-05-10 17:41:46,829 - INFO - worker - Listening on channels http.request, websocket.connect, websocket.disconnect, websocket.receive
2019-05-10 17:41:46,829 - INFO - worker - Listening on channels http.request, websocket.connect, websocket.disconnect, websocket.receive
2019-05-10 17:41:46,830 - INFO - worker - Listening on channels http.request, websocket.connect, websocket.disconnect, websocket.receive
2019-05-10 17:41:46,831 - INFO - server - HTTP/2 support not enabled (install the http2 and tls Twisted extras)
2019-05-10 17:41:46,831 - INFO - server - Using busy-loop synchronous mode on channel layer
2019-05-10 17:41:46,831 - INFO - server - Listening on endpoint tcp:port=8000:interface=127.0.0.1
[2019/05/10 17:43:16] WebSocket HANDSHAKING / [127.0.0.1:36462]
[2019/05/10 17:43:16] WebSocket CONNECT / [127.0.0.1:36462]
Hello

```
