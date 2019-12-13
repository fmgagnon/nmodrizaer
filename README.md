## nmodrizaer

Ce _patch_ Pure Data traite un signal sonore (originalement pour la guitare) en lui appliquant deux effets de volume : un tremolo et un hachoir. Ces effets sont modulée aléatoirement selon des paramètres réglables.

### Compatibilité

- Ce _patch_ n'a été testé qu'avec Pure Data pour macOS et avec PdParty pour iOS.

### Entrée/Sortie

- Le _patch_ additionne le signal des deux canaux de l'entrée système et émet le même signal dans les deux canaux de la sortie du système.

### Modes

- **Tremolo** : Applique une variation aléatoire dont l'amplitude et la fréquence de changement sont réglables.
- **Hachoir** : Coupe le signal si la valeur aléatoire (rafraîchie à une fréquence réglable) est supérieure à un seuil réglable.

### Contrôles

![Capture d'écran : contrôles](principal.png?raw=true "Contrôles")

- L'interrupteur **Tremolo** (en haut, à droite) active l'effet du tremolo aléatoire en multipliant le volume du signal par le paramètre aléatoire affiché juste à sa gauche. Ce paramètre est généré aléatoirement en fonction des réglages de gauche :
    - l'**amplitude** est définie de 0 à 100. À 100, le paramètre du tremolo varie de 0,5 à 1,5;
    - le **taux** de génération d'une valeur aléatoire est réglé en millisecondes.

- L'interrupteur **Hachoir** (en bas, à droite) active l'effet du hachoir en laissant passer le signal (post-tremolo, si le tremolo est activé) seulement si une valeur aléatoire (entre 0 et 100) est supérieure à un seuil réglable. L'indicateur à gauche de l'interrupteur représente le signal qui passe.
    - le **seuil** est réglable de 0 à 100. À 100, le signal est toujours bloqué;
    - le **taux** de génération d'une valeur aléatoire est réglé en millisecondes.

- Le réglage **mélange** représente la proportion du signal allant de 0 (seulement le signal d'origine, _dry_) à 1 (seulement le signal affecté, _wet_).

### Logique

![Capture d'écran : logique](logique.png?raw=true "Logique")

L'arithmétique est relativement simple :

- Pour le **tremolo**, le nombre aléatoire (entre 0 et X) est divisé par la moitié de X (X est l'amplitude réglée). Le résultat est divisé par 100, puis additionné à 1.
- Pour le **hachoir**, le seuil est soustrait du nombre aléatoire (entre 0 et 100). Si le résultat est négatif, il est forcé à 0.
