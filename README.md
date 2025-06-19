# Turn-Based RPG Combat System – FIIT STU LS 2024/25

**Autor:** Martin Kvietok  
**Predmet:** Database Systems  
**Fakulta:** Fakulta informatiky a informačných technológií, STU Bratislava  

## Popis projektu

Cieľom zadania bolo navrhnúť dátový model pre komplexný ťahový RPG súbojový systém. Model reflektuje herné mechaniky ako tvorba postavy, súboje, kúzla, inventár a logovanie akcií. Projekt simuluje správanie online RPG hier s dôrazom na modularitu, rozšíriteľnosť a konzistentnosť dát.

## Obsah systému

Projekt je rozdelený do logických balíkov podľa funkcionality:

### Character Package
- Definuje hráča a jeho atribúty: sila, obratnosť, odolnosť, inteligencia, zdravie.
- Triedy (`Class`) ovplyvňujú výpočet:
  - Max AP = (DEX + INT) × ClassModifier
  - Max Health = Health
  - Armor = 10 + (DEX/2) + bonus
  - Inventory Size = (STR + CON) × Modifier
- Status postavy: `InCombat`, `Resting`, `Idle`.
- Dynamické hodnoty: `CurrentHealth`, `CurrentAP`, `CurrentWeight`.

### Spell Package
- Kúzla majú vlastnosti: `BaseCost`, `BaseDamage`, `Accuracy`.
- Priradenie do kategórií (napr. oheň, ľad) s vlastnými modifikátormi.
- Používa `SpellAttributeEffect` pre ovplyvnenie výsledných hodnôt na základe hráčových atribútov.

### Inventory Package
- Predmety majú: `Name`, `Weight`, `Type`, `ItemModifier`.
- Stavy:
  - `InventoryItem` (v inventári)
  - `BattlegroundItem` (na bojisku)
- `isEquipped` určuje, či je predmet aktívne použitý.

### Combat Package
- Boje prebiehajú v kolách (`Round`), každé má začiatok a koniec.
- Účasť postavy: `CombatParticipant` (čas vstupu, stav).
- Logovanie udalostí: `CombatLog` (útoky, kúzla, získanie predmetu).

## Scenáre

1. **Zoslanie kúzla (útok, heal)**  
   - Výpočet nákladov a poškodenia podľa atribútov, hod kockou, úspech/nehod.

2. **Regenerácia a respawn postavy**  
   - Postava sa lieči v režime `Resting`. Po smrti sa resetujú hodnoty a začne znova.

3. **Pripojenie postavy do boja**  
   - Dynamické zapojenie do súboja, triedenie podľa `JoinTime`.

4. **Záznam útoku a efektov**  
   - Logovanie kúzla, cieľa, úspechu, výpočty `APCost` a `Damage`.

5. **Presun predmetov po smrti**  
   - Predmety sa automaticky presunú na bojisko (`BattlegroundItem`).

6. **Získanie predmetu z bojiska**  
   - Overenie nosnosti, priradenie predmetu, záznam v `CombatLog`.
