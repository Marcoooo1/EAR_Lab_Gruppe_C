# ORB-SLAM3 auf TUM RGB-D

Feature-basierter, sparse SLAM-Algorithmus mit Loop Closure, ausgeführt im RGB-D-Modus auf dem TUM RGB-D Datensatz.

## Umgebung

- Ubuntu 20.04 LTS unter WSL2
- Repository: https://github.com/UZ-SLAMLab/ORB_SLAM3

## Wichtige Anpassungen gegenüber dem Original-Repo

Zwei Stellen im CMakeLists.txt von ORB-SLAM3 mussten angepasst werden, damit der Build mit den unter Ubuntu 20.04 per apt verfügbaren Paketversionen durchläuft:

1. `find_package(OpenCV 4.4)` zu `find_package(OpenCV REQUIRED)` geändert, da apt nur OpenCV 4.2.0 bereitstellt.
2. C++-Standard von `c++11` auf `c++14` angehoben, weil die aktuelle Pangolin-Version (ihre interne sigslot-Bibliothek) C++14 voraussetzt.

Abhängigkeiten (Ubuntu-Pakete): build-essential, cmake, git, pkg-config, libgtk2.0-dev, libavcodec-dev, libavformat-dev, libswscale-dev, libeigen3-dev, libboost-serialization-dev, libssl-dev, libopencv-dev, python3-dev, python3-numpy, libgl1-mesa-dev, libglew-dev, libegl1-mesa-dev, libepoxy-dev.

Pangolin wurde als Tag v0.9.2 aus dem Quellcode gebaut (nicht der master-Branch, da dieser denselben C++14-Konflikt zusätzlich verschärft).

## Ausführung

```bash
./Examples/RGB-D/rgbd_tum \
    Vocabulary/ORBvoc.txt \
    Examples/RGB-D/TUM1.yaml \
    <Pfad zur Sequenz> \
    Examples/RGB-D/associations/fr1_xyz.txt
```

## Ergebnisse: Sequenz fr1_xyz

| Metrik | Wert |
| --- | --- |
| RMSE | 0.0124 m |
| Mean | 0.0110 m |
| Median | 0.0102 m |
| Std | 0.0056 m |
| Max | 0.0214 m |
| Min | 0.0023 m |

Berechnet mit `evo_ape` (SE(3) Umeyama-Alignment) gegen die Ground-Truth-Trajektorie der Sequenz.

Dateien in `results/fr1_xyz/`:

- `CameraTrajectory.txt`, `KeyFrameTrajectory.txt`: rohe Trajektorien im TUM-Format
- `ape_plot.pdf`: Fehler über die Zeit (Absolute Trajectory Error)
- `trajectory_overlay.pdf`: geschätzte Trajektorie gegen Ground-Truth im Overlay

## Reproduktion

```bash
evo_ape tum <groundtruth.txt> KeyFrameTrajectory.txt -va --plot_mode xyz --save_plot ape_plot.pdf
evo_traj tum KeyFrameTrajectory.txt --ref="<groundtruth.txt>" --plot_mode xyz --save_plot trajectory_overlay.pdf
```

## Ergebnisse: Sequenz fr1_desk

Längere, komplexere Sequenz mit Rückkehr zum Ausgangspunkt (Loop Closure relevant), im Gegensatz zu fr1_xyz.

| Metrik | ATE | RPE |
| --- | --- | --- |
| RMSE | 0.0192 m | 0.0173 m |
| Mean | 0.0163 m | 0.0138 m |
| Median | 0.0129 m | 0.0107 m |
| Std | 0.0101 m | 0.0105 m |
| Max | 0.0496 m | 0.0709 m |
| Min | 0.0035 m | 0.0028 m |

Dateien in `results/fr1_desk/`: `CameraTrajectory.txt`, `KeyFrameTrajectory.txt`, `ape_plot.pdf`, `rpe_plot.pdf`, `trajectory_overlay.pdf`.

## Vergleich der beiden Sequenzen

| Sequenz | ATE RMSE | RPE RMSE | Keyframes | Pfadlänge |
| --- | --- | --- | --- | --- |
| fr1_xyz | 0.0124 m | 0.0148 m | 61 | 7.0 m |
| fr1_desk | 0.0192 m | 0.0173 m | 121 | 9.3 m |

Beobachtung: trotz Loop Closure liegt der Fehler bei fr1_desk höher als bei fr1_xyz, vermutlich wegen der längeren, komplexeren Kamerabewegung und der größeren Anzahl an Keyframes. Für die Diskussion im Paper relevant: ob die Loop-Closure-Korrektur den zusätzlichen Drift durch die komplexere Trajektorie ausgleichen kann oder nicht.
