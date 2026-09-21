# Container Error Check

Een lichte, offline tool om snel de oorzaak te vinden van een container die in **error** staat bij het LP-receiving proces in Dynamics 365 F&O.

Je uploadt je Excel-exports en de tool spoort automatisch de gekende oorzaken op — geen server, geen installatie, alles draait lokaal in je browser.

## Wat het checkt

| Check | Kleur | Wat het betekent |
|-------|-------|------------------|
| **Duplicate references** | 🔴 | Meerdere license plates die naar dezelfde besteloregel wijzen. Vaak de oorzaak van *"inventory dimensions on load lines cannot be changed because work has been created"*. |
| **Batch vervallen** | 🔴 | Batchnummer met formaat `[land][JJJJMMDD]` waarvan de datum vandaag of eerder is (bv. `DE20260630`). Komt al over datum binnen → error. |
| **Batch zonder datum** | 🟠 | Batch bevat enkel een landcode (bv. `DE`, `AT`) of is leeg, terwijl hetzelfde item elders wél een datumcode heeft. |
| **Overdelivery** | 🟠 | Hoeveelheid boven de tolerantie t.o.v. *Deliver remainder* op de open PO-regel. |
| **Facturatie gestart** | 🔴 | PO-regel waar *Quantity* > *Deliver remainder*: er is al een deel ontvangen en gefactureerd. D365 weigert dan elke boeking die het regelbedrag raakt — ook als het aantal keurig past. Oorzaak van *"You are attempting to enter values that will change the source document amounts..."*. Dit is een **hard feit**, geen gewogen gok: de tool toont hem bovenaan met de melding *feit — geen gok*. Vereist export 2. |

## Welke exports

1. **License plate / Packaging export** (verplicht)
   Licenseplate receiving sessions → open de errored sessie → Packaging structure details → tabblad *Items* → Export to Excel.

2. **Open purchase order lines export** (nodig voor overdelivery-% én voor de check *facturatie gestart*)
   Inquiry *Open purchase order lines* → filter op *Overdelivery* = 10 → Export to Excel.

De tool herkent de kolommen automatisch (Item number, Purchase order, Quantity, Batch number, Reference, Deliver remainder, License plate) — je hoeft niets te hernoemen.

## Gebruiken

Open **[de tool](https://Jens-1985.github.io/container-error-check/)** in je browser, sleep je export erin, klaar.

Of lokaal: download `index.html` en open het met dubbelklik. Werkt volledig offline.

## Privacy

Er wordt **niets geüpload**. Alle verwerking gebeurt in je eigen browser (via SheetJS). De Excel-bestanden verlaten je computer niet.

## Roadmap

- [ ] Recv-Out / reservations-check (crossdock-botsingen)
- [ ] Prioriteitsvolgorde: "test deze items eerst" bij manuele ontvangst
- [ ] Serienummer-verplicht-maar-leeg check

---

Gemaakt voor intern warehouse-gebruik. Groeit mee met elke nieuwe foutsoort.
