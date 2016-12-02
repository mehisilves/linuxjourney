﻿# Tuuma paigaldamine

## Tunni sisu

Nüüd kui kogu see igav värk on selja taga, räägime parem tuuma paigaldamisest ja muutmisest. Süsteemi võib paigaldada mitu tuuma. On veel meeles peatükk alglaadimisest? Alglaaduri GRUB menüüst saab valida, millise tuumaga süsteem käivitada.

Et tuvastada süsteemi tuuma versioon, kasuta käsku:

<pre>$ uname -r
3.19.0-43-generic</pre>

*Uname* käsk kuvab süsteemi informatsiooni, -r kuvab tuuma väljalaskeversiooni.

Linuxi tuuma võib paigaldada mitut moodi. Võib laadida alla lähtepaketi ja kompileerida, aga seda võib teha ka kasutades paketihaldusvahendeid ja paigaldades alati uusimast tuuma versioonist sõltuvad metapaketid - need uuenevad automaatselt ehk siis kogu süsteemi uuendades (*sudo apt update && sudo apt dist-upgrade && sudo apt clean*) uueneb ka tuum koos päistega uusima versiooni peale.

**NB! Peale tuuma uuendamist tuleb alati ka käivitada alglaaduri uuendamise käsku _sudo update-grub_, et teavitada uuema tuuma paigaldamisest - siis teab alglaadur GRUB ka uusimat tuuma kasutusele võtta järgmise taaskäivitamise ajal.**

Linuxi tuuma ja päise metapakettide paigaldamine:<br>
<pre>$ sudo apt install linux-image-generic-lts-xenial linux-headers-generic-lts-xenial linux-image-generic linux-headers-generic && sudo apt clean && sudo update-grub</pre>

Seejärel arvuti taaskäivitada värskelt paigaldatud tuumaga. On ju lihtne? Täpsustada saab ka versiooninumbrit, ülemine käsk võib välja näha ka nii kuigi see ei ole kõige parem mõte kuna sõltub konkreetsest tuuma ja selle päise versioonist, mis ei uuene (kavalam on paigaldada eespool kirjeldatud automaatselt uuenevad metapaketid): <b>sudo apt install linux-image-4.4.0-51-generic linux-image-extra-4.4.0-51-generic linux-image-generic linux-headers-4.4.0-51 linux-headers-4.4.0-51-generic linux-headers-generic</b>.

Alternatiivina, kui huvi on uuendatud versiooni järgi, võib kasutada *dist-upgrade*'i. See teostab uuendused kõikidele pakettidele. Ei tohi unustada ka eespool kirjeldatud alglaaduri uuendamist uusima tuuma kasutuselevõtmiseks. Linuxi tuum uueneb pidevalt ja sellega kaasneb parem riistvara tugi, vigade parandused ja seeläbi ka parem turvalisus, kogu süsteemi parem töötamine. Seetõttu on oluline kasutada võimalikult uut tuuma ja vanad eemaldada.

Kui metapaketid paigaldatud siis edaspidi toimub uusimate tuumade ja päiste paigaldus automaatselt kogu süsteemi uuendades:<br>
<pre>$ sudo apt update && sudo apt dist-upgrade && sudo apt clean</pre>

On olemas palju erinevaid tuuma versioone, näiteks LTS (*long term support* ehk pikaajalise toega) ja on ka muidugi veel uuemaid ning vingemaid. Versioonide vahel võib olla väga palju erinevusi ja võib juhtuda, et kasutaja tahab proovida erinevaid tuumasid. Üldiselt võib alati ka uusimaid versioone proovida - ka need töötavad ja lisavad üldjuhul uusima riistvara tuge ja parandatud saavad ka turvavead. Eriti just turvalisuse seisukohast lähtuvalt võiks süsteemis kasutada alati uusimat tuuma ja eemaldada vanad tuumad.

Mõistlik on otsida uusimad LTS-versiooni metapaketid ja paigaldada need - siis süsteemi uuendamise (*dist-upgrade*) käigus uuendatakse alati uusima versiooni pikaajalise toega (LTS) tuuma peale:

<pre>
$ sudo apt search linux-image-generic-lts
$ sudo apt search linux-headers-generic-lts
</pre>

Leitud tulemustest tuleks paigaldada viimane ehk siis uusim ja olla nõus ka sõltuvustena uusimate tuuma ja selle päiste versioonide paigaldamisega. Uusima versiooni saab teha kindlaks kasutatava Linuxi koodnime järgi, siin toodud näide on Ubuntu Linuxi kohta ja see peaks kehtima ka teiste Debiani ja Ubuntu baasil tehtud Linuxite kohta. Ei tohi unustada ka eespool kirjeldatud alglaaduri uuendamist uusima tuuma kasutuselevõtmiseks.

Ubuntu koodnimed on toodud siin tabelis. Oma Ubuntu koodnime vaatamiseks:
<pre>
lsb_release -cd
</pre>

Seda näeb veel ka näiteks *cat /etc/lsb-release* abil.

Kui uusim LTS-versiooni tuum paigaldatud, alglaadur uuendatud, süsteem taaskäivitatud ja veendutud *uname -r* abil, et töötatakse uusima tuuma pealt siis tuleks vanad ja kasutuses mitteolevad tuumad turvalisuse kaalutlustel ka eemaldada.

Vaatame esmalt, millised tuumad on paigaldatud:
<pre>
$ dpkg --get-selections | grep linux-image
<pre>
või ka
<pre>
$ dpkg -l | grep linux-image
<pre>
Paigaldatud tuumi, päiseid näeb ka kui vaadata kataloogi *ls -l /boot* - tuum on nimega *vmlinuz* ja teised sama versiooninumbriga moodustavadki tuuma komplekti koos päise ja kõige muu juurdekuuluvaga.

RPM-põhistes Linuxites näeb paigaldatud tuumi käsuga *rpm -q kernel*

Seejärel veendume, millise tuuma versiooni pealt arvuti hetkel töötab: *uname -r*. Kui töötab uusima pealt siis võib kohe asuda vanu eemaldama, vastasel korral tuleb uuendada alglaadurit ja arvuti taaskäivitada: *sudo update-grub && sudo reboot*

Vanade tuumade eemaldamiseks on uuematel Ubuntu versioonidel olemas võimalus:
<pre>
$ sudo purge-old-kernels
</pre>
... see eemaldab kõik vanad tuumad ja jätab alles kaks viimast.

Kui soovitakse alles jätta vaid viimane siis:
<pre>
$ sudo purge-old-kernels --keep 1 -qy
</pre>
*--keep 1* hoiab alles vaid viimase tuuma versiooni<br>
*-q* teeb ilma väljundit näitamata (*quiet*)<br>
*-y* on vaikimisi nõus.<br>
Lisainfot leiab *man purge-old-kernels*

Kuid vaikimisi sellist mugavat vanade tuumade eemaldamise käsku ei ole - selleks tuleb paigaldada pakett *[byobu](http://byobu.co/)*, mis on [olemas ka Ubuntu vaikimisi varamutes](http://packages.ubuntu.com/search?keywords=byobu&searchon=names&suite=all&section=all) ent mitte kõige uuem. Uusima saab kui lisada [vastav varamu](https://launchpad.net/~byobu/+archive/ubuntu/ppa):
<pre>
sudo add-apt-repository ppa:byobu/ppa && sudo apt-get update
</pre>
... ning paigaldada byobu
<pre>
sudo apt update && sudo apt install byobu && sudo apt clean
</pre>
Lisaks vanade tuumade eemaldamisele võimaldab byobu palju muudki, lisainfot leiab [programmi kodulehelt](http://byobu.co/).

<b>Käsitsi vanade tuumade eemaldamine.</b>
Kui ei ole võimalik rakenduse byobu'ga kaasatulevat mugavat võimalust vanade tuumade kasutamiseks rakendada siis saab ka käsitsi neid eemaldada.<br>
Paketihaldusest otsime paigaldatud tuumi ja nende päiseid:
1.variant (paigaldatud pakettide tuvastamine):
<pre>
dpkg-query -l 'linux-image*' | grep '^ii'<br>
dpkg-query -l 'linux-header*' | grep '^ii'
</pre>

Graafiliselt tööjaamas sama otsing:
otsida (CTRL+F) Synaptic'uga pakette:
*linux-image*<br>
*linux-header*
.. ja eemaldada siis need, mis on vanemad ja millelt masin ei tööta, pannes tühikutega eraldades need argumentidena järgmiselt:
<pre>
sudo apt purge <pakk 1> <pakk 2>.....<pakk n>
</pre>

**NB! Enne eemaldamist veenduda _uname -r_ abil, milliselt tuuma versioonilt masin töötab.**

Võib kasutada ka mõnda teist paketihaldustarkvara, soovi korral saab Synaptic'u ka paigaldada:
<pre>
sudo apt update && sudo apt-get -y install synaptic && sudo apt clean
</pre>

2.variant otsimiseks
<pre>
sudo dpkg --get-selections | grep linux-image<br>
sudo dpkg --get-selections | grep linux-header
</pre>

<b>Uusim tuum ja päised Ubuntule</b>
On võimalik paigaldada ka päris uusi versioone kuid neid ei saa automaatselt uuendada ja selle lahenduse valimisel tuleb ka edaspidi käsitsi uuendada ja ise regulaarselt jälgida kui uus versioon välja tuleb. Kui minna ametlikule Linuxi tuuma kodulehele https://www.kernel.org/ siis näeb, mis on hetkel uusim tuuma versioon (*stable*). Sealt näeb ka teisi tuumade versioone, mida veel toetatakse (*longterm*) ja mis on uusim arenduses olev versioon (*mainline*).

Uusimad Ubuntu jaoks pakendatud tuumad ja päised leiab aadressilt http://kernel.ubuntu.com/~kernel-ppa/mainline/ - tuleb minna lehekülje lõppu (nt klahviga *End*) ja võtta suurima numbriga kataloog. Vältida tuleks katalooge, mille nime lõpus on *rc1*, *rc2* jne - need on veel arendusjärgus olevad väljalaskekandidaadid (*rc - release candidate*).

Paigaldamiseks tuleb teada ka oma süsteemi arhitektuuri, üldiselt kas 32-bit või 64-bit: *uname -m*. Kui vastuseks on *i686* (või ka *i386*) siis 32-bit ja kui *x86_64* siis on 64-bit süsteemiga tegemist.

Soovitav on alla laadida eraldi kataloogi, näiteks */home/kasutaja/Allalaadimised/tuum/* kataloogi.

32-bit süsteemi korral tuleb alla laadida:<br>
<pre>
linux-headers-VERSIOONINUMBER_VERSIOONINUMBER.LOOMISE-AEG_all.deb
linux-headers-VERSIOONINUMBER-generic_VERSIOONINUMBER.LOOMISE-AEG_i386.deb
linux-image-VERSIOONINUMBER-generic_VERSIOONINUMBER.LOOMISE-AEG_i386.deb
</pre>

Kui on saadaval siis võib võtta ka *linux-image-extra-VERSIOONINUMBER_amd64.deb*

Tuumad, mille nimes on *lowlatency* on reaalaja tuumad ja mõeldud teistsugustele süsteemidele kus vaja väga kiiret reageerimist, mida kasutatakse näiteks helitöötluses, arvutijuhitavate tööpinkide juhtimiseks jne. Seal on saadaval veel ka teistele riistvara arhitektuuridele tuumi (*arm*, *ppc* jne).

64-bit süsteemi korral tuleb alla laadida:<br>
<pre>
linux-headers-VERSIOONINUMBER_VERSIOONINUMBER.LOOMISE-AEG_all.deb
linux-headers-VERSIOONINUMBER-generic_VERSIOONINUMBER.LOOMISE-AEG_amd64.deb
linux-image-VERSIOONINUMBER-generic_VERSIOONINUMBER.LOOMISE-AEG_amd64.deb
</pre>

Enne paigaldamist tuleb ka veenduda, et kataloogis */home/kasutaja/Allalaadimised/tuum/* ei ole muid mittevajalikke *.deb* faile ega ka vanu, juba paigaldatud tuumade ja päiste faile - kui on siis need tuleks enne järgmise paigalduskäsu käivitamist sealt kataloogist kustutada.

Allalaaditud tuuma ja päise paigaldamiseks:
<pre>
cd /home/kasutaja/Allalaadimised/tuum/ && sudo dpkg -i && sudo update-grub *
</pre>

Peale uusima tuuma paigaldamist ja GRUB'i uuendamist tuleb süsteem taaskäivitada ja eemaldada vanad tuumad, millest eespool ka juttu oli. Kui vanade tuumade eemaldamisel sõltuvusena eemaldatakse ka pakett *linux-generic* siis selles ei ole midagi halba ja selle võib eemaldada.

## Harjutus

<ol>
<li> Uurida välja enda operatsioonisüsteemi tuuma versioon.</li>
<li> Uurida erinevate saadaval olevate tuumade kohta</li>
</ol>

## Küsimus

Kuidas saab teada tuuma versiooni?

## Vastus
*uname -r*