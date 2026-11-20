# OpenCode — reviewer local independent

Pachet minim pentru a adăuga un **reviewer independent local** într-un proiect OpenCode: agentul are voie doar să citească fișierele, nu le modifică, și verifică implementarea față de cerințele originale ale utilizatorului.

> Acesta este doar configul + workflow-ul. Modelul local propriu-zis (serverul PolyStrata/llama.cpp, configurile VRAM, paznicul) nu face parte din acest repo.

## Fișiere

| Fișier | Rol |
|---|---|
| `.opencode/opencode.json` | Definește agentul `reviewer` ca subagent, cu model local și permisiuni `edit` / `write` / `patch` / `shell` refuzate. |
| `AGENTS.md` | Workflow-ul obligatoriu de 7 pași: capturarea cerințelor, implementare, auto-verificare, descoperirea modelului, review independent, buclă de corecție, status final. |
| `README.md` | Acest fișier. |

## Instalare

1. Copiază `.opencode/opencode.json` în rădăcina proiectului tău (pestește configul existent sau adaugă doar cheia `agents.reviewer`).
2. Copiază `AGENTS.md` în rădăcina proiectului, sau integrează secțiunea „Mandatory workflow for development tasks" în AGENTS.md existent.
3. Schimbă `model` din config cu modelul tău local de review. În workspace-ul de origine s-a folosit `polystrata/gemma-4-26b-a4b-qat-ud-q4_k_xl`, servit local prin PolyStrata. Dacă folosești alt provider/model, actualizează atât `model`, cât și ultima linie din `AGENTS.md`.
4. Asigură-te că modelul e disponibil pentru OpenCode înainte de a rula review-ul; altfel statusul va fi `◐ Local AI Unavailable`.

## Cum funcționează

- La fiecare sarcină de dezvoltare, agentul principal trece prin cei 7 pași din `AGENTS.md`.
- Reviewerul e un `subagent` proaspăt (`agent: "reviewer"`), deci nu are contextul implementării și nu poate modifica fișierele (permisiunile `edit`, `write`, `patch`, `shell` sunt `deny`).
- Primește ca dovezi cerințele originale **verbatim** și implementarea reală, și raportează findings pe severitate, cu dovezi concrete din fișierele citite.
- Dacă apar probleme confirmate, se corectează, se re-verifică și se trimite un reviewer nou până nu mai rămân probleme.

## Statusuri de verificare

- `✓ Verified` — auto-verificare trecută, reviewer complet și valid, zero probleme confirmate.
- `◐ Partially Verified` — verificări incomplete sau dovezi insuficiente.
- `◐ Local AI Unavailable` — endpointul/modelul local nu e disponibil.
- `✗ Verification Failed` — probleme confirmate nerezolvate sau teste eșuate.

## Origine

Extras din workspace-ul `three js`. Nu conține codul jocului, asset-uri sau tokeni și secrete.
