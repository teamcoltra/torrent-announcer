# BitTorrent Tracker Announcer

[![Build Status](https://travis-ci.org/Haehnchen/torrent-announcer.png)](https://travis-ci.org/Haehnchen/torrent-announcer)

BitTorrent Tracker Announcer can request a BitTorrent Tracker for Torrent details. You can then output eg the peerlist or the seeder/leecher count.

## Requirements
requires PHP 5.3.x or above. The recommended version is 5.3.2 or newer.

## Installation
You can install it with Composer.

### Composer
Simply specify `espend/torrent-announcer` in your dependencies.

## Using
### Autoloader

BitTorrent Tracker Announcer does **not** come with its own autoloader, so you will need to use a PSR-0 compatible autoloader for everything to work as expected, or provide your own `require[_once]` statements. An example of such an autoloader can be found [here](https://gist.github.com/1234504).

### Send request to Tracker

```php
<?php
$req = TorrentRequest::createOnTorrent(\PHP\BitTorrent\Torrent::createFromTorrentFile('yout torrent file.torrent'));
$req->setTorrentClient(new \BitTorrent\Announcer\Client\uTorrentClient());
echo $req->announce()->render(); // output: d8:completei211e10:inc[...]
print_r($req->announce()->getPeers()); // output: peerlist array with host/id
```

### Proxy Request
Mainly for developing you can overwrite the announce url on your favorite bittorent client to pipe to a custom server which than will do the request as a proxy. just use the announce as a base64 encoded parameter

```php
<?php
#index.php?announce=aHR0cDovL3RvcnJlbnQudWJ1bnR1LmNvbTo2OTY5L2Fubm91bmNl
$req = TorrentRequest::createFromRequestArray(PlainTorrentClient::createFromGlobals());
echo $req->announce()->render(); // output: d8:completei211e10:inc[...]
```

### Clients
Every BitTorrent uses his own announce Parameters. Here some common prefined classes for simulate the different Requests to a Tracker. You can also implement new one with the help of `TorrentClientInterface`.

```
PlainTorrentClient
BitTornadoClient (TorrentFlux)
TransmissionClient
uTorrentClient
```

### Parameters
All BitTorrent announce Parameter are mapped to different classes to handle them in a better way.
```
BitTorrent\Announcer\Request
 - announce_url

BitTorrent\Announcer\RequestParamter
 - info_hash
 - uploaded
 - downloaded
 - left
 - event
 
BitTorrent\Announcer\Client\Abstracts\TorrentClientInterface
 - peer_id
 - compact
 - no_peer_id
 - port
 - numwant
 - key
 - (User-Agent, Header)
```