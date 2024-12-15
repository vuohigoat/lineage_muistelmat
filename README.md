# LineageOS 18.1 buildaus potterille

Kone: Macbook pro mid-2012 RAM 16GB

Käyttis: Ubuntu 24.04 (Dualboot MacOS kanssa) tallennustilaa 200GB

Aloitin lukemalla lineagen virallista ohjetta credricille. [https://wiki.lineageos.org/devices/cedric/build/](https://wiki.lineageos.org/devices/cedric/build/). Ohjeet eivät ole potterille, mutta ovat kaikille laitteile käytännösssä samat ja credric on lähimpänä potteria. Latasin repon avulla lineageOS 18.1 source koodit ja laitoin ne lataamaan yön yli. Aamulla heräsin ikävään yllätykseen, repo-käsky ei ollut ajanut onnistuneesti netin katkeamisen takia. No, ei se mitään, ajoin repo sync -f --force-broken, joka siis asentaa rikkoutuneet tiedostot uudestaan. Kun source oli ladattu, aloin miettiä, mistä löytäisin proprietary tiedostot potterille. Minun tuli siis löytää vendor, kernel ja device tree. Käytin potter-telegram ryhmän suositusten mukaan CRdroidin tiedostoja lukuun ottamatta kerneliä, jonka löysin N00bkerneliltä. Linkit:

Vendor: [https://github.com/crdroidandroid/android_vendor_motorola_potter/tree/11.0](url) 

Device: [https://github.com/crdroidandroid/android_device_motorola_potter/tree/11.0](url) 

Kernel: [https://github.com/N00bKernel/potter](url)

Ensiksi yritin CRdroidin motorola_msm8953-kerneliä, mutta kun aloin buildata päätyi buildi erroriin, sillä tuossa ei ollut potterin tiedostoja. Laitoin nämä kaikki linkit siis local_manifest.xml -nimiseen tiedostoon, josta repo sync lataa tiedostot. Yritin jälleen kokeilla buildausta, mutta päädyin taas erroriin, olin unohtanut laittaa android_system_qcom -tiedoston manifestiin. Jälleen kerran muokkasin manifestin ja ajoin repo sync. Esimerkkiä manifestin muodosta otin zjunior06-nimiseltä github-käyttäjältä. [https://github.com/zjunior06/local_manifests/blob/master/local_manifest.xml](url) 

No niin, vihdoin tuli uusi errori buildatessa! En ollut seurannut lineagen ohjeita tarpeeksi tarkkaan ja olin unohtanut asentaa libcurses5-nimisen kirjaston. Noh, ei se mitään. Ajetaan vain sudo apt install libncurses5 ja kaikki on hyvin. Näin ei tietenkään ollut, vaan libncurses5 on poistettu Ubuntun oletussourcesta. Noin tunnin googlailun kälkeen löysin sopivat mirrorit, joista tuon voi asentaa. [https://askubuntu.com/questions/1531398/how-to-install-libncurses-so-5-for-ubuntu-24-04](url) Ajoin siis roottina seuraavan käskyn:
```
echo "deb http://security.ubuntu.com/ubuntu focal-security main universe" > /etc/apt/sources.list.d/ubuntu-focal-sources.list
```
Sen jälkeen päivitin sourcet:
```
sudo apt-get update
```
Ja pystyin vihdoin asentamaan kirjaston:
```
sudo apt-get install libncurses5
```
Jes, vihdoin kaikki on kunnossa ja pääsin oikeasti buildaamaan!
