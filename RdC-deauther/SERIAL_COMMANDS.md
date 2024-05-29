# Serial Commands

**Overview:**

- [`help`](#help)
- [`scan [<all/aps/stations>] [-t <time>] [-c <continue-time>] [-ch <channel>]`](#scan)
- [`show [selected] [<all/aps/stations/names/ssids>]`](#show)
- [`select [<all/aps/stations/names>] [<id>]`](#select-deselect)
- [`deselect [<all/aps/stations/names>] [<id>]`](#select-deselect)
- [`add ssid <ssid> [-wpa2] [-cl <clones>] [-f]`](#add-ssid)
- [`add ssid -ap <id> [-cl <clones>] [-f]`](#add-ssid)
- [`enable random <interval>`](#random)
- [`disable random`](#random)
- [`remove <ap/station/name/ssid> <id>` ](#remove)
- [`remove <ap/station/names/ssids> [all]` ](#remove)
- [`attack [beacon] [deauth] [deauthall] [probe] [nooutput] [-t <timeout>]`](#attack)
- [`attack status [<on/off>]`](#attack-status)
- [`sysinfo`](#sysinfo)
- [`clear`](#clear)
- [`format`](#format)
- [`get <setting>`](#get)
- [`set <setting> <value>`](#set)
- [`reset`](#reset)
- [`stop <all/scan/attack>`](#stop)
- [`chicken`](#chicken)
- [`reboot`](#reboot)
- [`info`](#info)
- [`send deauth <apMac> <stMac> <rason> <channel>`](#send)
- [`send beacon <mac> <ssid> <ch> [wpa2]`](#send)
- [`send probe <mac> <ssid> <ch>`](#send)

## HELP

`help`
Stampa l'elenco dei comandi

## SCAN

`scan [<all/aps/stations/wifi>] [-t <time>] [-c <continue-time>] [-ch <channel>]`
**Inizia la scansione di tutti gli AP's nel raggio d'azione**
**Modes** (optional, default = all):

- all (-a)
- aps (-ap)
- stations (-st)
- wifi (-w)

## SHOW

`show [<all/aps/stations/names/ssids>]`
`show selected [<all/aps/stations/names/ssids>]`
Stampa a schermo tutti gli SSID trovati
**Selected** (optional, default = false): Solo quelli selezionati vengono stampati.
**Types** (optional, default = all):

- all (-a)
- aps (-ap)
- stations (-st)
- names (-b)
- ssids (-ss)

## SELECT-DESELECT

`select [<all/aps/stations/names>] [<id>]`
`deselect [<all/aps/stations/names>] [<id>]`
(De)Selects seleziona o deseleziona un SSID o un dispositivo in particolare.
**Types** (optional, default = all):

- all (-a)
- aps (-ap)
- stations (-st)
- names (-b)

**ID** (optional): ID of the AP/station/device you want to select.

## ADD-SSID

`add ssid <ssid> [-wpa2] [-cl <clones>] [-f]`
Aggiunge un SSID.
**ssid**: nome dell'SSID che vuoi aggiungere.
**-wpa2** (optional, default = false): flag per impostare o meno la sicurezza WPA2/PSK.
**clones** [-cl] (optional, default = 0): Quanti cloni vuoi aggiungere.
**force** [-f] (optional, default = false): Forzatura ad aggiungere i cloni fino a che il buffer non è pieno.

`add ssid -ap <id> [-cl <clones>] [-f]`
Copia gli SSID di una rete scelta.
**clones** [-cl] (optional, default = 0): Quantità di cloni.
**force** [-f] (optional, default = false): Forzatura ad aggiungere i cloni fino a che il buffer non è pieno.

## RANDOM

`enable random <interval>`
Abilità la generazione random di APs per un numero di secondi pari ad interval.

`disable random`
Disabilita la random mode.

## REMOVE

`remove <ap/station/name/ssid> <id>`
Rimuove uno specifico SSID o dispositivo dall'elenco.
**Types**:

- ap (-ap)
- station (-st)
- name (-n)
- ssid (-ss)

`remove <ap/station/names/ssids>`
`remove <ap/station/names/ssids> [all]`

**all** [-a] (optional, default = true): Rimuove tutto.

## ATTACK

`attack [beacon] [deauth] [deauthall] [probe] [nooutput] [-t <timeout>]`
Inizia il tipo di attacco indicato [attenzione, bisogna fornire almeno un parametro]

- **beacon** [-b] (optional, default = false)
- **deauth** [-d] (optional, default = false)
- **deauthall** [-da] (optional, default = false)
- **probe** [-p] (optional, default = false)
- **nooutput** [-no] (optional, default = false): Disabilita il debug dei pacchetti/secondo.
- **timeout** [-t] (optional, default = settings.attackTime): Cambia il timeout di ogni attacco.

## ATTACK STATUS

`attack status [<on/off>]`
Abilita o disabilita il debug dello stato dell'attacco in corso.

## SYSINFO

`sysinfo`
Stamp la RAM, il canale WiFi corrente, il MAC addresses, SPIFFS (SPI-Flash-File-System).

## CLEAR

`clear`
Effettua una clear della console seriale.

## FORMAT

`format`
Elimina tutti i file presenti nella SPIFFS (SPI-Flash-File-System).

## GET

`get <setting>`
Effettua una get dell'impostazione `<setting>`

## SET

`set <setting> <value>`
Setta il valore di `<setting>` a `<value>`.

## RESET

`reset`
Resets delle impostazioni ai valori di default.

## STOP

`stop [<all/scan/attack>]`

Stop .**Modes** (optional, default = all):

- all (-a)
- scan (-sc)
- attack (-a)

## CHICKEN

`chicken`
Piccolo easteregg.

## REBOOT

`reboot`
hard reset (cortocircuito tra reset e groung pin GPIO).

## INFO

`info`
Stampa info di sistema e link della repository.

## SEND

`send deauth <AP-MAC> <ST-MAC> <reason> <channel>`
`send beacon <mac> <ssid> <ch> [wpa2]`
`send probe <mac> <ssid> <ch>`

Invia un pacchetto per volta del tipo selezionato

## DELAY

`delay <time>`
Pausa della serial console per un periodo di tempo pari a `<time>`.
`delay 1000`  pausa di un secondo.
