# magnet2torrent

This module uses [torrent-stream](https://www.npmjs.com/package/torrent-stream) to download metadata of magnet links only, without creating any temp file.

## Install

```bash
npm i magnet2torrent-js
```

## API

### `m2t = new Magnet2torrent(options)`

Optional options are:

```javascript
{
    trackers = [],                  // {Array<string>} Trackers to use, default is []
    addTrackersToTorrent = false,   // {boolean} Default is false
    timeout = 60                    // {number} Timeout seconds, set to 0 will disable, default is 0
}
```

### `m2t.getTorrentBuffer(magnet)`

`magnet` can be a magnet link `magnet:?xt=urn:btih:90289fd34dfc1cf8f316a268add8354c85334458`  
or a hash `90289fd34dfc1cf8f316a268add8354c85334458`

## Example

### Download torrent file simply

```javascript
const Fs = require('fs');
const Magnet2torrent = require('magnet2torrent-js');

// ubuntu-16.04.1-server-amd64.iso
const magnet = 'magnet:?xt=urn:btih:90289fd34dfc1cf8f316a268add8354c85334458';

const m2t = new Magnet2torrent({
    timeout: 60
});

m2t.getTorrentBuffer(magnet).then(buffer => {
    Fs.writeFileSync('test.torrent', buffer);
}).catch(e => {
    console.error('Timeout or error occured');
});
```

### Use custom trackers and write them to torrent file

```javascript
const Fs = require('fs');
const Magnet2torrent = require('magnet2torrent-js');

// https://github.com/ngosang/trackerslist
const trackers = [
    'udp://tracker.coppersurfer.tk:6969/announce',
    'udp://tracker.open-internet.nl:6969/announce',
    'udp://tracker.leechers-paradise.org:6969/announce',
    'http://tracker3.itzmx.com:6961/announce',
    'http://tracker1.itzmx.com:8080/announce',
    'udp://bt.xxx-tracker.com:2710/announce'
];

// ubuntu-16.04.1-server-amd64.iso
let magnet = 'magnet:?xt=urn:btih:90289fd34dfc1cf8f316a268add8354c85334458';

let m2t = new Magnet2torrent({
    trackers,
    addTrackersToTorrent: true
});

m2t.getTorrentBuffer(magnet).then(buffer => {
    Fs.writeFileSync('test.torrent', buffer);
});
```
