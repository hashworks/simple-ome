# Simple Frontend for OvenMediaEngine

Provides a simple configuration and frontend for [OME](https://github.com/AirenSoft/OvenMediaEngine).

## Installation

OME, nginx and NPM are required.

The AUR provides a [PKGBUILD for Arch Linux](https://aur.archlinux.org/packages/ovenmediaengine): `yay -Syu ovenmediaengine`

```bash
mkdir -p /usr/local/src
cd /usr/local/src
git clone https://github.com/hashworks/simple-ome.git
cd simple-ome

cp Server.xml /etc/ovenmediaengine

cp nginx_vhosts.conf /etc/nginx/sites-available/stream.example.net.conf

cd frontend
npm install
```

Adjust `/etc/ovenmediaengine/Server.xml` and `/etc/nginx/sites-available/stream.example.net.conf` according to your liking.

## Required ports

| Port        | Protocol | Description            |
| ----------- | -------- | ---------------------- |
| 1935        | TCP      | RTMP Ingest            |
| 443         | TCP      | nginx (TLS)            |
| 3478        | TCP      | WebRTC Relay           |
| 10000-10005 | UDP      | WebRTC Ice             |

## Frontend

Provides a HTML5 web-frontend with the [OvenPlayer](https://github.com/AirenSoft/OvenPlayer) and a stream key that is adjustable by the URL fragment/hash (f.e. `https://stream.example.net#customkey`, defaults to `public`).

![frontend_with_custom_key](.images/frontend_with_custom_key.png)

## Authentication

Use the [`signed_policy_generator.sh`](https://github.com/AirenSoft/OvenMediaEngine/blob/master/misc/signed_policy_url_generator.sh) script to generate a signed policy for authenticated providers:

```
$ bash signed_policy_url_generator.sh thisisatestkey rtmp://stream.example.net:1935/live/public signature policy '{"url_expire":7258165200000}'
[URL] rtmp://stream.example.net:1935/live/public?policy=eyJ1cmxfZXhwaXJlIjo3MjU4MTY1MjAwMDAwfQ&signature=kl7-vm0hRn-ROay1Ms-A0qb4Kok
[Percent encoded URL] rtmp%3A//stream.example.net%3A1935/live/public%3Fpolicy%3DeyJ1cmxfZXhwaXJlIjo3MjU4MTY1MjAwMDAwfQ%26signature%3Dkl7-vm0hRn-ROay1Ms-A0qb4Kok
```

![obs setting with signed policy](.images/obs_setting_with_signed_policy.png)
