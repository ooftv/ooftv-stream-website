QUICK NOTES:

Reload the config:
sudo nginx -t && sudo nginx -s reload

Restart the server:
sudo brew services restart denji/nginx/nginx-full

Tail the logs:
tail -f /usr/local/var/log/nginx/access.log
tail -f /usr/local/var/log/nginx/error.log

Refresh the certs:

Refresh duckdns IP:

Validate an HLS stream:
Check if the oof stream is in compliance with the HLS spec:
you'll need mediastreamcalidator and hlsreport from developer.apple.com


>> mediastreamvalidator https://www.ooftv.net/live/oof.m3u8

since it's a live stream you'll need to stop it with ctrl-c

the results won't be super useful, probably everything will look find. it will also output a vcalidation_data.json

>> hlsreport validation_data.json

this creates a validation_data.html, open it in a browser



for RTMP / HLS Streaming to work, you need 3 pieces.

why rtmp? why hls? (to be filled in)

1) You need a video source. This could be a video signal from a camera or hdmi - or it could be a file, or even an rtmp stream

2) You need an encoder. Ideally you have a handful of variants. So you need several encoders, or a strong machine. keep in mind, the encoding will take energy and cause heat, so that has to be accounted for.

3) You need a web server to display it all. The webserver us just copying files and serving them to requestors. It shouldn't use processing for video encoding.

Ideally all of these 3 pieces are from at least 3 different pieces of hardware. But theoretically it could all run on one machine.

===============
Architect Idea:

Pull method


streaming server publishes stream
encoder 1 uses ffmpeg to pull stream from encoder and encodes it, then re-publishes it via rtmp
stream 2 does the same thing
http server pulls from encoders and servers to the web






once the stream is working, use HTTPLiveStreamingTools from the Apple Developer website to validate the stream
mediastreamvalidator https://ooftv.duckdns.org/live/index.m3u8

logs are here:
access_log /usr/local/var/log/nginx/access.log;
      error_log  /usr/local/var/log/nginx/error.log debug;


-----------------------------
How to find the stream to address, for encoder or OBS
------------------------------

Sample:
rtmp://ooftv.duckdns.org/stream/oof

to figure out what address the encoder should send the video to, it's
rtmp://
followed by the server address
rtmp://ooftv.duckdns.org
followed by the RTMP application name defined in nginx.conf
rtmp://ooftv.duckdns.org/stream
followed by the key - which names the files. so up to hear everything is configured in the nginx.conf. but this part is the encoder's choice
rtmp://ooftv.duckdns.org/stream/oof
