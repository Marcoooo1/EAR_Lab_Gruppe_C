# TODO: SLAM-Vergleich EAR Lab Gruppe C

Damit unsere vier Implementierungen am Ende wirklich vergleichbar sind, hier die verbindlichen Vorgaben. Bitte genau so umsetzen, sonst lassen sich die Ergebnisse im Paper nicht gegenüberstellen.

## 1. Datensatz (für alle gleich)

TUM RGB-D, zwei Sequenzen:

- **fr1_xyz**: kurze, einfache Sequenz, kein Loop Closure
- **fr1_desk**: längere Sequenz mit Rückkehr zum Ausgangspunkt, Loop Closure relevant

Download:

    wget https://vision.in.tum.de/rgbd/dataset/freiburg1/rgbd_dataset_freiburg1_xyz.tgz
    wget https://vision.in.tum.de/rgbd/dataset/freiburg1/rgbd_dataset_freiburg1_desk.tgz

Beide Sequenzen mit dem eigenen Algorithmus laufen lassen, nicht nur eine.

## 2. Algorithmus-Zuteilung

| Person | Algorithmus | Status |
| --- | --- | --- |
| Marco | ORB-SLAM3 | fertig, siehe orb_slam3/ |
| ? | RTAB-Map | offen |
| ? | DSO | offen |
| ? | ElasticFusion | offen |

Bitte in dieser Datei eintragen wer welchen Algorithmus macht.

## 3. Metriken (für alle gleich)

Mit evo (pip install evo --upgrade --user) berechnen:

    evo_ape tum <groundtruth.txt> <eure_trajektorie.txt> -va --plot_mode xyz --save_plot ape_plot.pdf
    evo_rpe tum <groundtruth.txt> <eure_trajektorie.txt> -va --plot_mode xyz --save_plot rpe_plot.pdf
    evo_traj tum <eure_trajektorie.txt> --ref="<groundtruth.txt>" --plot_mode xyz --save_plot trajectory_overlay.pdf

Wichtig: Beide Metriken (ATE und RPE) für beide Sequenzen, nicht nur eine. Bei RTAB-Map und ElasticFusion zusätzlich eine Aufnahme/einen Screenshot der erzeugten dichten Karte/Rekonstruktion, das brauchen wir für die Kartenvergleich-Abbildung im Paper.

## 4. Ablagestruktur im Repo

Pro Person ein eigener Ordner nach dem Schema `<algorithmus>/results/<sequenz>/`, siehe `orb_slam3/` als Vorlage:

    <algorithmus>/
      README.md
      results/
        fr1_xyz/
          <trajektorie>.txt
          ape_plot.pdf
          rpe_plot.pdf
          trajectory_overlay.pdf
        fr1_desk/
          ... (gleiche Dateien)

## 5. README pro Algorithmus (Pflichtinhalt)

- Kurzbeschreibung des Funktionsprinzips (feature-basiert/direkt, sparse/dense, mit/ohne Tiefensensor)
- Build-Hinweise, insbesondere Abweichungen vom Original-Repo (Versionsanpassungen, Patches)
- Ergebnistabelle: ATE und RPE (RMSE, Mean, Median, Std, Max, Min) für beide Sequenzen
- Kurze Beobachtung/Interpretation der eigenen Zahlen

Orientierung: `orb_slam3/README.md` im Repo.

## 6. Visualisierungsanforderungen laut Angabe (nicht vergessen)

- Mindestens ein Plot mit allen vier Trajektorien im Overlay (kommt am Schluss, wenn alle Daten da sind)
- Mindestens eine Karte/Rekonstruktion pro Implementierung
- Alle Abbildungen im Paper referenziert und beschriftet
- Einheitliche Serifenschrift wie im Paper, mindestens Fließtextgröße, mind. 300 DPI, auch in Graustufen lesbar
- Aussagekräftiger Titel und selbsterklärende Caption pro Abbildung

## 7. Deadline

Bitte Ergebnisse spätestens [DATUM EINTRAGEN] auf dem eigenen Branch pushen, damit wir rechtzeitig vor dem nächsten Lab-Termin den gemeinsamen Vergleich und das Paper zusammenstellen können.
