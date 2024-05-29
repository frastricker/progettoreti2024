# RDC - Deauther

## Progetto di Francesco Maura

Il progetto in questione nasce all'incirca 6 anni fa da una mia curiosità sul mondo della sicurezza informatica ed
Ethical Hacking. Il progetto è stato poi fermo per qualche tempo finché non ho deciso di rispolverarlo per questo progetto
di Reti di Calcolatori. Molte funzioni le ho scritte io basandomi su alcuni contributi che ho apportato ad un altro progetto su GitHub del 2018 (eliminato purtroppo) ed altre le ho prese da un progetto di Deauther sotto licenza MIT.
Il presente codice è TUTTO sotto licenza MIT (vedere il file LICENSE)

## Descrizione

Un deauther Wi-Fi è un dispositivo che sfrutta una vulnerabilità nel protocollo 802.11 per interrompere le connessioni wireless tra dispositivi e punti di accesso (access points). Questa interruzione viene ottenuta inviando pacchetti di deautenticazione (deauth packets). Di seguito è riportata una spiegazione tecnica dettagliata del funzionamento di un deauther Wi-Fi, del protocollo di rete utilizzato, del tipo di pacchetto inviato e della vulnerabilità sfruttata.

Il Deauthing è un processo mediante il quale un terminale si disconnette da un canale di connessione. In questo caso la connessione è di tipo WiFi e il Deauthing avviene mediante una vulnerabilità di alcuni router.

Il beacon flooding è un processo con cui il dispositivo crea un numero arbitrario di reti. Questo tipo di "attacco"  non è un vero e proprio attacco, in quanto posso clonare un SSID con le relative configurazioni di rete mantenendo però tutti i dispositivi connessi alla rete originale. Questo tipo di attacco se condotto in singolo permette agli utenti non autenticati alla rete di trovare "difficoltà" nel connettersi al vero AP in quanto con questo attacco se ne possono generare fino a 2^32 -2 cloni. Se condotto insieme all'attacco di deauthing il beacon flooding permette di bloccare la connessione presso l'AP originale e "costringere" l'utente a connettersi ad una rete fasulla.

Un possibile continuo di questo tipo di attacco (che non è stato ancora implementato ma lo verrà in futuro) è predisporre un reindirizzamento dell'AP fasullo sull'Internet Globale cambiando i record DNS del dispositivo attaccante facendo credere alla vittima di essere collegato ad una rete legittima. Con questo inganno si può utilizzare il DNS Spoofing che permette di reindirizzare l'utente vittima a siti clone di quello originale permettendone il furto delle informazioni.

## Funzionamento

Il funzionamento del deauther si bassa sull'utilizzo dell'ESP8266. L' **ESP8266** è un microchip Wi-Fi, con stack TCP / IP completo e funzionalità di **microcontrollore.**

#### Protocollo di Rete Utilizzato: IEEE 802.11

Il protocollo IEEE 802.11 è lo standard per le reti wireless (Wi-Fi). Gestisce la comunicazione tra dispositivi wireless e punti di accesso. Questo protocollo include meccanismi per autenticazione, associazione e gestione delle connessioni.

#### Tipo di Pacchetto Inviato: Pacchetti di Deautenticazione


I pacchetti di deautenticazione sono un tipo di frame di gestione nel protocollo 802.11. Questi pacchetti sono utilizzati per terminare la connessione di un dispositivo con un punto di accesso. I pacchetti di deautenticazione contengono le seguenti informazioni principali:

* **Frame Control:** Identifica il tipo di frame (in questo caso, un frame di gestione).
* **Duration:** Specifica la durata della trasmissione.
* **Destination Address:** L'indirizzo MAC del dispositivo di destinazione.
* **Source Address:** L'indirizzo MAC del punto di accesso.
* **BSSID:** L'indirizzo MAC del punto di accesso che gestisce la rete.
* **Sequence Control:** Numero di sequenza del frame.
* **Reason Code:** Un codice che specifica il motivo della deautenticazione.

## Processo


* **Scansione delle Reti:** Il deauther Wi-Fi esegue una scansione per individuare le reti Wi-Fi disponibili (SSID) e i dispositivi connessi (client).
* **Selezione dei Target:** L'utente seleziona i dispositivi o i punti di accesso da deautenticare.
* **Invio dei Pacchetti di Deautenticazione:** Il deauther invia una serie di pacchetti di deautenticazione al dispositivo di destinazione o al punto di accesso. Questi pacchetti possono essere inviati a:
  * **Client specifici:** L'indirizzo di destinazione è l'indirizzo MAC del client.
  * **Tutti i dispositivi:** Utilizzando l'indirizzo di broadcast, tutti i dispositivi connessi alla rete possono essere deautenticati.

## Vulnerabilità Sfruttata


Il deauther Wi-Fi sfrutta una vulnerabilità intrinseca nel protocollo 802.11: la mancanza di autenticazione nei pacchetti di deautenticazione. Questo significa che chiunque, anche senza autorizzazione, può inviare pacchetti di deautenticazione e far sì che i dispositivi si disconnettano dalla rete. Le principali caratteristiche della vulnerabilità sono:

* **Assenza di Autenticazione:** I pacchetti di deautenticazione non richiedono alcun tipo di autenticazione, quindi possono essere inviati da qualsiasi dispositivo che si trovi entro il raggio della rete Wi-Fi.
* **Interruzione della Connessione:** Quando un dispositivo riceve un pacchetto di deautenticazione, considera la connessione interrotta e tenta di ricollegarsi, causando una disconnessione temporanea.
* **Denial of Service (DoS):** L'invio continuo di pacchetti di deautenticazione può causare un attacco DoS, impedendo ai dispositivi di mantenere una connessione stabile.

### Misure di Mitigazione

Per proteggersi dagli attacchi di deautenticazione, si possono adottare le seguenti misure:

* **Utilizzo di WPA3:** WPA3 offre una protezione migliorata contro questo tipo di attacchi attraverso l'autenticazione dei pacchetti di gestione.
* **Monitoraggio della Rete:** Implementare sistemi di rilevamento delle intrusioni (IDS) per identificare e mitigare attacchi di deautenticazione.
* **Aggiornamenti del Firmware:** Utilizzare access point e router con firmware aggiornato che supporti le misure di sicurezza avanzate.

In conclusione, un deauther Wi-Fi sfrutta una debolezza nel protocollo 802.11 per inviare pacchetti di deautenticazione non autenticati, interrompendo le connessioni wireless. Comprendere il funzionamento di questi attacchi e adottare misure preventive è essenziale per mantenere la sicurezza delle reti Wi-Fi.

## Esempio in linguaggio C

Per inviare pacchetti di deautenticazione in C, è possibile utilizzare la libreria `libpcap`, che permette la cattura e l'invio di pacchetti di rete. Di seguito è riportato un esempio di codice in C che invia pacchetti di deautenticazione a un indirizzo MAC:

```
#include <pcap.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>

#define MAC_ADDR_LEN 6

void send_deauth_packet(pcap_t *handle, const uint8_t *target_mac, const uint8_t *gateway_mac) {
    uint8_t deauth_packet[26] = {
        0x00, 0x00, // Frame Control
        0x00, 0x00, // Duration
        0xff, 0xff, 0xff, 0xff, 0xff, 0xff, // Destination (broadcast)
        0x00, 0x00, 0x00, 0x00, 0x00, 0x00, // Source (will be set later)
        0x00, 0x00, 0x00, 0x00, 0x00, 0x00, // BSSID (will be set later)
        0x00, 0x00, // Sequence Control
        0x07, 0x00  // Reason Code (7: Class 3 frame received from nonassociated STA)
    };

    // Set Source MAC and BSSID to gateway MAC
    memcpy(deauth_packet + 10, gateway_mac, MAC_ADDR_LEN);
    memcpy(deauth_packet + 16, gateway_mac, MAC_ADDR_LEN);

    // Send the packet multiple times
    for (int i = 0; i < 100; ++i) {
        if (pcap_sendpacket(handle, deauth_packet, sizeof(deauth_packet)) != 0) {
            fprintf(stderr, "Error sending the packet: %s\n", pcap_geterr(handle));
            return;
        }
        usleep(100000); // Sleep for 100ms
    }
}

int main() {
    char errbuf[PCAP_ERRBUF_SIZE];
    pcap_t *handle;

    const char *dev = "wlan0"; // Network interface
    const uint8_t target_mac[MAC_ADDR_LEN] = {0xAA, 0xBB, 0xCC, 0xDD, 0xEE, 0xFF}; // Target MAC (invented)
    const uint8_t gateway_mac[MAC_ADDR_LEN] = {0x11, 0x22, 0x33, 0x44, 0x55, 0x66}; // Gateway MAC (invented)

    handle = pcap_open_live(dev, BUFSIZ, 1, 1000, errbuf);
    if (handle == NULL) {
        fprintf(stderr, "Couldn't open device %s: %s\n", dev, errbuf);
        return 2;
    }

    send_deauth_packet(handle, target_mac, gateway_mac);

    pcap_close(handle);
    return 0;
}

```

### Spiegazione del Codice

* **Inclusione delle librerie necessarie:** `pcap.h` per l'uso di `libpcap`, `stdio.h` per le funzioni di input/output, `stdlib.h` per la gestione della memoria, `string.h` per le funzioni di manipolazione delle stringhe, `unistd.h` per le funzioni di sistema come `usleep`.
* **Definizione delle costanti:** `MAC_ADDR_LEN` per la lunghezza degli indirizzi MAC.
* **Funzione `send_deauth_packet`:**
  * Crea un pacchetto di deautenticazione con un frame control vuoto, una durata vuota e un indirizzo di destinazione broadcast (ff:ff:ff:ff:ff
    ).
  * Imposta gli indirizzi MAC di origine e BSSID all'indirizzo del gateway.
  * Invia il pacchetto 100 volte con un intervallo di 100ms tra ogni invio.
* **Funzione `main`:**
  * Configura l'interfaccia di rete `wlan0` per la cattura live dei pacchetti.
  * Imposta gli indirizzi MAC di destinazione e del gateway.
  * Chiama la funzione `send_deauth_packet` per inviare i pacchetti di deautenticazione.
  * Chiude la sessione di `pcap`.

### Avvertenza

L'invio di pacchetti di deautenticazione è illegale in molte giurisdizioni se non viene effettuato su reti di tua proprietà o senza l'autorizzazione esplicita del proprietario della rete. Usa questo codice solo a fini educativi e legali.

## Visibilità dell'AP attaccante

Il seguente frammento di codice permette di collegarsi da remoto e fornisce le credenziali per accedere.
L'ho creato per due motivi:

1. Individuare l'attaccante (per una questione legale, si può settare invisibile da riga di comando seriale)
2. Collegarsi al dispositivo per implementare un futuro WebServer
