# Reakční hra s Ř-duino-LED
Cílem tohoto projektu bude vytvořit jednoduchou hru pro dva hráče, která bude mít za úkol otestovat váš reakční čas. Bude se skládat ze dvou částí: tři LEDky a dvě tlačítka. Po spuštění programu začne prostřední LEDka blikat. Poté co třikrát blikne se na náhodně dlouhou dobu zhasne. Jakmile se opět rozsvítí, začíná soutěž o to, kdo stiskne své tlačítko jako první. Stiskne-li jeden z hráčů své tlačítko před tím, než se LEDka opět rozsvítí, automaticky prohraje. Po skončení hry se rozsvítí vítězova LEDka. Budete-li si přát hru opakovat, je možné ji restartovat stisknutím kovového tlačítka RESTART v levé horní části desky.
Pokud jste si na desku nahráli jiný kód, původní program je ke stažení [zde](https://github.com/prokyber/Rduino-LED)

# Součástky
- Ř-duino-LED
- [tlačítka](https://e-shop.prokyber.cz/spinace/mikrospinac-12-12-7-3mm/)  2x
- [rezistory](https://e-shop.prokyber.cz/pasivni-soucastky/rezistor/)  2x
- [vodiče](https://e-shop.prokyber.cz/kabely--vodice/dupont-kabel/)  7x
- [nepájivé pole](https://e-shop.prokyber.cz/konektory/nepajive-pole-170-pinu/)

# Rezistor

![Obrázek rezistorů](https://github.com/prokyber/r-duino-led-reaction-speed/blob/main/img/Screenshot%20from%202024-05-21%2008-14-17.png)

Rezistor je pasivní elektrická součástka, která se v obvodu projevuje zvýšením odporu. Nejčastěji se do obvodu přidává za účelem snížení proudu na potřebnou úroveň. Pro výpočet funkce jednotlivého rezistoru můžeme použít Ohmův zákon (proud = napětí / odpor), avšak chceme-li použít rezistorů víc, je třeba je správně "zařadit". Pro bližší pochopení tohoto tématu si můžete přečíst třeba tento tutoriál.

# Tlačítko

<table>
  <tr align="center">
    <td>
      <img alt="Rezistor" src="https://github.com/prokyber/r-duino-led-reaction-speed/blob/main/img/FKQ6Q43LUQYQ0NW.webp" style="Height: 34vh;">
    </td>
    <td>
      <img alt="Rezistor" src="https://github.com/prokyber/r-duino-led-reaction-speed/blob/main/img/Tlacitko_Schema.webp" style="Height: 34vh;">
    </td>
  </tr>
  <tr>
    <td colspan="2">
      <img alt="Rezistor" src="https://github.com/prokyber/r-duino-led-reaction-speed/blob/main/img/F09ZYSRLUQYQ0NT.webp" style="Height: 51vh;">
    </td>
  </tr>
</table>

Tlačítko je jednou ze základních elektrických součástek. Jeho funkci snad již znají úplně všichni. Jak ale takové tlačítko vlastně funguje? Po stisknutí spojí obvod a umožní průchod elektrického proudu skrz tlačítko. Můžeme si ho představit jako lávku na laně, která přemosťuje dvě cesty. Poté, co tlačítko stiskneme, "lávka" se sníží, a "cestující" (elektřina) tak po ní mohou přejít. 

Pro lepší představu se můžete podívat na obrázek s bílým pozadím, černými čarami a kruhy. Toto je způsob, jak se tlačítko zobrazuje na schématech elektrických obvodů, a přijde mi, že celkem dobře popisuje jeho fungování. 

Jedna věc, na kterou si dát pozor, je, že naše tlačítka mají čtyři nožičky, z nichž dvě jsou vždy propojené v páru. Na obrázku s černou a červenou čárou je naznačeno, které dvě nožičky jsou vždy propojeny, a chceme-li, aby nám proud procházel z červené čáry na černou, pak musíme tlačítko stisknout. Pomůckou nám může být, že vždy jsou propojené nožičky, které jsou přímo naproti sobě. 

V sadě vždy dodáváme pět tlačítek s hmatníky (ty barevné čepičky), jež jdou na tlačítko nasadit pro pohodlnější používání. 

# Pull-down Rezistor

| <img alt="Obrázek pulldown rezistoru" src="https://github.com/prokyber/r-duino-led-reaction-speed/blob/main/img/Pulldown_rezistor.webp" style="Height: 50vh;" /> | <img alt="Obrázek pullup rezistoru" src="https://github.com/prokyber/r-duino-led-reaction-speed/blob/main/img/Pullup_rezistor.webp" style="Height: 50vh;" /> |
| ---- | ---- |

Při čtení z pinů se občas setkáváme s problémem, kdy se aktivuje funkce, kterou jsme však nezamýšleli spustit. Toto se děje v důseldku šumu, který může vzniknout na nožičce, pokud není připojena k uzemnění. Proto je nutné připojit tzv. pull-down rezistor na tlačítko (viz obrázek nalevo). Tím zajistíme, že na nožičce bude nízké napětí, což nám pomůže eliminovat šum. Rezistor musí být zapojen, aby nedošlo ke zkratu, když stiskneme tlačítko. 

Existuje také tzv. Pull-up rezistor (viz obrázek napravo). Zde se princip obrátí a do pinu nám prochází proud, dokud tlačítko není stisknuto. Poté proud přesměrujeme do uzemnění a na nožičce tak zůstane nízké napětí. 

Pro ty, kteří jsou seznámeni s tímto konceptem, bych také rád zmínil, že desky Ř-duino disponují vnitřními pull-up rezistory a lze jej použít pomocí syntaxe: 

pinMode(PIN, INPUT_PULLUP). 

# Princip Funkce
Po zapnutí programu se spustí funkce zodpovědná za blikání LEDky. Tlačítka jsou připojena na tzv. Interrupt piny, které jsou schopny paralelně s během programu vnímat změny napětí. Po stištění libovolného tlačítka se spustí funkce zodpovědná za odpojení tlačítek, ukončení hry a vyhodnocení vítěze. 

# Zapojení

<img alt="Rezistor" src="https://github.com/prokyber/r-duino-led-reaction-speed/blob/main/img/Reakcni_zapojeni.webp" style="Height: 51vh;">

(Šedé čáry značí nožičky rezistoru)


Pin A5 je naším programem určen jako napájecí pin. Používáme ho tak proto, abychom zamezili situacím kdy jeden z hráčů stiskne tlačítko jen o chlup později než ten druhý a tím by v programu mohlo dojít k neočekávaným vzorcům chování. Proto jakmile dojde ke stisknutí kteréhokoliv z tlačítek, se nám okamžitě vypne napájecí pin a tlačítka tak přestanou plnit svou funkci.

Propojíme tedy vodičem A5 a libovolný sloupek nepájivého pole, čímž si vytvoříme takovou křižovatku odkud pak následně budeme vést proud ke tlačítkům.


Poté co proud dorazí ke tlačítku bude se moct po jeho překročení (stištení) vydat do dvou stran. Jedna ho povede na jeden ze čtecích pinů (pin 2/ pin 3), ta druhá ho pak přes rezistor povede k uzemění. Díky tomuto způsobu zapojení nám vznikne výše zmiňovaný pull-down rezistor.


K uzemění budeme tlačítka potřebovat propojit s pinem GND. Toho lze dosáhnout více způsoby, avšak ten který je znázorněn na obrázku nám přišel nejpraktičtější.


LEDky pro tento projekt zapojovat nemusíme, jsou totiž už vestavěnné v desce a ovládány programem.

# Nastavení Desky

<img alt="Nastavení desky" src="https://github.com/prokyber/r-duino-led-reaction-speed/blob/main/img/Reakcni_deska_nastaveni.webp" style="Height: 30vh;">

Před tím než desku zapojíte do počítače, nastavte zabudovaný DIP switch na hodnotu ukázanou na obrázku. (světlé části jsou vystouplé výběžky přepínače) Poku jste již desku zpojili, je možné ji resetovat kovovým tlačítkem v levém horním rohu. Po zapojení by se měl program spustit.

Budete-li chtít hru opakovat stačí zmáčknout výše zmíněné tlačítko RESTART.
