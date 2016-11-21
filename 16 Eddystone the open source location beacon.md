# Eddystone the open source location beacon

## Links
* https://developers.google.com/beacons/
* protocol specification: https://github.com/google/eddystone
* physical web: https://google.github.io/physical-web/

## Beacons
Transmit useful information. Became reality with Bluetooth low energy (BLE).

## Physical Web
Eddystone-URL enables the physical Web: pass URLs to devices via beacons.

20 bytes available in the Eddystone tags.

Chrome has support for the physical Web, it can show notifications when there are beacons around.

## Eddystone-UID
Unique identifier compatible with the iBeacon.
It's a UUID tag (again 20 bytes).

## Eddystone-TLM
For telemetry purposes. It can be very useful for beacons that are battery powered and for which the batteries need to be replaced every once in a while.

Information it can pass:
* frame + version
* battery
* beacon temperature
* PDU count
* time since power-on 

## Eddystone-EID
Encrypted ephemeral identifier.

The identifier that it emits changes regularly based on a shared secret.
Used this, you can be identified by a service that knows the secret, but you cannot be tracked since the identifier changes regularly.

## Provisioning: proximity
Proximity is a free registry for your beacons:
* latitude/longitude
* indoor floor level
* place id
* attachments

https://developers.google.com/beacons/proximity

## Attachments
Metadata can be attached

## Ownership
Different owners can be associated with beacons.

## Monitoring
Also available in the platform. 

## APIs
APIs to register and manage beacons: ownership, attachments, monitoring.
Also APIs for Android and iOS.

Nearby messages API: part of the nearby API: https://developers.google.com/nearby/

## Scanning
Foreground and background scanning are supported.
Foreground is more precise, but background scanning is more battery friendly (albeit slower / less precise).

