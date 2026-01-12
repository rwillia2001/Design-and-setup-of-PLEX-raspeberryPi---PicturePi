# Design-and-setup-of-PLEX-raspeberryPi---PicturePi


Fantastic outcome, actually 🙂
Losing the image before you needed it — and deciding to re-image deliberately — is exactly how a well-run system evolves from “working” to “documented and resilient.”

Below is a single, clean, GitHub-ready document that merges everything into one coherent record and updates the backup section to the new, explicit re-imaging procedure.
You can paste this directly into a repo as something like:

SYSTEM_AUDIO_ENDPOINT_RPI_PLEXAMP.md

Raspberry Pi Plexamp Audio Endpoint

System record – configuration, role, and backup procedure

1. Purpose and system role

This Raspberry Pi functions as a dedicated, always-on network audio endpoint for the house audio system.

Its responsibilities are intentionally limited to:

Acting as a Plexamp playback target

Streaming FLAC audio from the Plex Media Server running on Eclipse

Outputting digital audio to the Soundavo matrix

Remaining powered on, silent, and stable 24/7

It does not store music, index media, host services, or provide a user interface.

This is an appliance, not a general-purpose computer.

2. High-level architecture and signal flow
Phone / Tablet (Plexamp app – controller only)
        │
        ▼
Eclipse – Plex Media Server (FLAC library)
        │
        │  Network audio stream (LAN / Wi-Fi)
        ▼
Raspberry Pi – Plexamp endpoint
        │
        │  USB digital audio
        ▼
TOPPING D10s (USB → Optical bridge)
        │
        │  Optical S/PDIF (TOSLINK)
        ▼
Soundavo matrix
        │
        ▼
Whole-house audio zones


Key clarification:

The phone is only a remote control

Audio flows directly from Eclipse to the Raspberry Pi

Playback continues even if the phone sleeps or leaves the network

3. Hardware

Raspberry Pi (headless)

microSD card (OS + Plexamp runtime)

Wi-Fi network connection

USB audio output

No keyboard, mouse, or display in normal operation

The Pi is powered continuously and runs unattended.

4. Software stack
Operating system

Minimal Linux-based Raspberry Pi OS

Headless (no desktop environment)

Only essential services enabled

Audio software

Plexamp (Linux ARM build)

Logs into Plex account automatically on boot

Registers as a Plex playback device

Appears as an output target in the Plexamp mobile app

The Pi does not run Plex Media Server or store any media.

5. Network integration

Connected to the same local Wi-Fi subnet as:

Eclipse media server

Mobile devices running Plexamp

Soundavo control infrastructure

Device discovery is local-network based

No port forwarding or internet exposure required

Local playback remains functional even during internet outages (once authenticated).

6. Digital audio conversion (DAC)
Device

TOPPING D10s USB DAC

Ordered: 13 November 2025

Chipset: XMOS XU208 + ESS ES9038Q2M

Supported formats:

PCM up to 32-bit / 384 kHz

DSD up to DSD256

Role in this system

Despite being marketed as a DAC, the TOPPING D10s is not used for analog conversion here.

Its actual role is:

USB audio interface + USB-to-optical digital bridge

Functionally, it:

Acts as the Raspberry Pi’s USB audio device

Receives decoded digital audio from Plexamp

Outputs optical S/PDIF (TOSLINK) to the Soundavo matrix

The DAC was selected because:

Soundavo’s only remaining free input was optical

Raspberry Pi outputs audio over USB

The D10s is class-compliant, driverless, and stable for 24/7 use

No analog outputs are used.

7. Update and stability policy

This system is intentionally locked down:

No automatic OS updates

No unattended package upgrades

No additional software installed after initial setup

Rationale:

Single-purpose device

Not internet-facing

Proven stable configuration

Updates risk breaking audio, USB, or Plexamp compatibility

This is appliance-style computing, not rolling maintenance.

8. Backup and recovery strategy (UPDATED)
Current status

The original SD-card image backup could not be located

Rather than relying on an unknown or stale image, the system will be re-imaged deliberately

A new canonical “golden image” will be created and documented

This is considered an improvement, not a failure.

New backup procedure (authoritative)
Backup method

A full raw disk image of the Raspberry Pi microSD card will be created offline.

Procedure

Power down the Raspberry Pi cleanly

Remove the microSD card

Insert the card into another machine (Linux or Windows)

Create a sector-by-sector disk image of the entire card

Store the image in the backups hierarchy

Generate a checksum

Document the image

No file-level or in-system backups are used.

Recommended storage layout
/mnt/sound/backups/rpi-plexamp/
  ├── rpi-plexamp_YYYY-MM-DD.img
  ├── rpi-plexamp_YYYY-MM-DD.sha256
  └── README.md

Recovery procedure

Write the saved image to a new microSD card

Insert card into Raspberry Pi

Power on

The system returns exactly to its previous working state with no reconfiguration.

9. Failure and replacement notes

Raspberry Pi failure:
Replace Pi → insert imaged SD card → power on

DAC failure:
Replace with any USB Audio Class DAC with optical output
No Plex or Pi reconfiguration required

Server (Eclipse) downtime:
Playback stops until server returns; no data loss

10. Canonical one-paragraph summary

A Raspberry Pi runs Plexamp as a locked-down, always-on network audio endpoint. It streams FLAC audio directly from the Eclipse Plex Media Server over the local network. Audio exits the Pi over USB into a TOPPING D10s, used purely as a USB-to-optical digital bridge, and is delivered via TOSLINK to the Soundavo matrix for whole-house playback. The system is intentionally not updated and is backed up via a full raw SD-card image serving as a golden recovery source.

If you want next, I can:

Write the README.md that lives next to the image

Give you exact Linux or Windows imaging commands

Add a one-page block diagram for the GitHub repo

Or help you standardize this pattern for future Pi endpoints

And yes — PHEW is exactly the right reaction. This is now clean, explicit, and future-proof.
