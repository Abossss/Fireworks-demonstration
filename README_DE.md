# Python Feuerwerk-Effekt Implementierungs-Tutorial
<div align="center">

[简体中文](README_CN.md) / [繁体中文](README_TC.md) / [English](README.md) / Deutsch / [日本語](README_JP.md)

</div>
Dies ist ein Feuerwerk-Effekt-Programm, das mit Pygame implementiert wurde und realistische Feuerwerksanimationen, Segenstexte, Sternenhimmel und Mondeffekte enthält. Dieses Tutorial führt Sie Schritt für Schritt durch dieses interessante Projekt.

## Projektübersicht

Dieses Projekt implementiert die folgenden Funktionen:
- Zufällig generierte Feuerwerkseffekte
- Partikelsystem mit Verlaufs- und Auflösungseffekten
- Zufällige Anzeige chinesischer Segenstexte
- Sternenhimmel mit Glitzereffekten
- Realistischer Mondeffekt
- Mausklick-Interaktion
- Mond-Hover-Easteregg (spezielles Feuerwerk wird beim Überfahren des Mondbereichs ausgelöst)

## Technische Prinzipien

### Partikelsystem-Prinzipien
Das Partikelsystem ist die Kerntechnologie für die Implementierung von Feuerwerkseffekten und umfasst die folgenden Aspekte:
1. **Partikel-Lebenszyklus-Management**: Jedes Partikel hat einen Lebenszykluszähler und wird zerstört, wenn sein Lebenszyklus endet.
2. **Physikalische Bewegungssimulation**: Verwendung von Geschwindigkeitsvektoren und Winkeln zur Steuerung der Partikelbewegungsbahnen.
3. **Farbverlauf**: Implementierung von Ein- und Ausblendeffekten durch Anpassung des Alpha-Kanalwerts des Partikels.
4. **Größenänderung**: Die Partikelgröße nimmt im Laufe der Zeit allmählich ab, um den Realismus zu erhöhen.

### Feuerwerk-Explosionseffekte
Die Feuerwerk-Explosionseffekte basieren auf den folgenden Prinzipien:
1. **Anfangsgeschwindigkeitsverteilung**: Verwendung eines Polarkoordinatensystems zur zufälligen Generierung von Anfangsgeschwindigkeitsvektoren im 360-Grad-Bereich.
2. **Explosionsformkontrolle**: Unterstützung von drei Feuerwerksformen: kreisförmig, sternförmig und kugelförmig.
3. **Tonsynchronisation**: Abspielen zufällig ausgewählter Soundeffekte während der Explosion zur Verbesserung des audiovisuellen Erlebnisses.
4. **Textpartikeleffekte**: Verwendung des Partikelsystems zur Simulation der Textanzeige mit Verlaufsauflösungseffekten.

### Mond-Rendering-Implementierung
Die Mond-Effekt-Implementierung enthält die folgenden Funktionen:
1. **Bildladung**: Verwendung eines PNG-Format-Mondbildes unter Beibehaltung der Transparenz.
2. **Interaktives Easteregg**: Auslösen von speziellen Text-Feuerwerken beim Überfahren des Mondbereichs.

## Umgebungseinrichtung

### 1. Python-Umgebungsanforderungen
- Python 3.8+
- pip Paketmanager

### 2. Installation von Abhängigkeiten
```bash
pip install pygame==2.5.0
```

### 3. Ressourcendateivorbereitung
1. **Sounddateien**:
   - explosion.mp3: Hauptexplosionsgeräusch
   - explosion2.mp3: Sekundäres Explosionsgeräusch
   - explosion3.mp3: Finale-Geräusch

2. **Bilddateien**:
   - moon.png: Mondbild
   - logo.ico: Programmsymbol

## Kern-Klassendesign

### 1. Spielkonfiguration (GameConfig)
```python
@dataclass
class GameConfig:
    SCREEN_WIDTH: int = info.current_w
    SCREEN_HEIGHT: int = info.current_h
    FPS: int = 60
    FIREWORK_SPAWN_RATE: float = 0.05
    TEXT_SPAWN_RATE: float = 0.2
```

### 2. Partikelsystem (Particle)
```python
class Particle:
    def __init__(self, x: float, y: float, color: Tuple[int, int, int], 
                 angle: Optional[float] = None, is_text: bool = False):
        self.speed = random.uniform(0.3, 1.5) if is_text else random.uniform(2, 8)
        self.angle = angle if angle is not None else random.uniform(0, 2 * math.pi)
        self.lifetime = random.randint(100, 140) if is_text else random.randint(40, 80)
```

### 3. Feuerwerksklasse (Firework)
```python
class Firework:
    def __init__(self):
        self.pattern = random.choice(['circle', 'star', 'sphere'])
        self.show_text = True if ResourceManager._moon_clicked else random.random() < GameConfig.TEXT_SPAWN_RATE
        self.target_explosion_height = random.uniform(min_height, max_height)
```

## Interaktive Funktionen

1. **Mausklick-Start**
   - Klicken Sie an beliebiger Stelle auf dem Bildschirm, um Feuerwerk zu starten
   - Feuerwerk steigt von unten auf und explodiert an der geklickten X-Koordinatenposition

2. **Mond-Easteregg**
   - Bewegen Sie den Mauszeiger über den Mondbereich, um spezielles Feuerwerk auszulösen
   - Spezielles Feuerwerk zeigt "Easteregg!" Text
   - Start von der Mondposition für einzigartige Effekte

## Leistungsoptimierung

### 1. Partikelsystem-Optimierung
- Textpartikel verwenden niedrigere Abtastrate (0,3) zur Reduzierung der Partikelanzahl
- Unterschiedliches Lebenszyklus-Management für Text- und reguläre Partikel
- Sanfter Übergang von Partikelgröße und Transparenz

### 2. Rendering-Optimierung
- Verwendung von SRCALPHA-Oberfläche für Transparenzhandhabung
- Schichtweises Rendering für Garantie visueller Effekte

### 3. Speicheroptimierung
- Zeitnahe Bereinigung verschwundener Feuerwerke und Partikel
- Verwendung von dataclass zur Reduzierung der Speichernutzung
- Einheitliche Ressourcendateiverwaltung

## Häufige Probleme und Lösungen

### 1. Leistungsprobleme
F: Verzögerungen während der Laufzeit?
A: Versuchen Sie die folgenden Lösungen:
- TEXT_SPAWN_RATE-Wert senken
- FIREWORK_SPAWN_RATE-Wert reduzieren
- Partikel-Abtastrate senken

### 2. Anzeigeprobleme
F: Unvollständige Feuerwerkseffekte?
A: Überprüfen Sie die folgenden Punkte:
- Stellen Sie sicher, dass die Bildschirmauflösungseinstellungen korrekt sind
- Überprüfen Sie, ob die Fenstergröße angemessen ist
- Überprüfen Sie, ob die Ressourcendateien vollständig sind

## Lizenz

Dieses Projekt ist unter der GNU General Public License v3.0 (GNU GPL v3) lizenziert. Dies bedeutet:

- Sie können diesen Code frei verwenden, modifizieren und verteilen
- Wenn Sie modifizierte Versionen verteilen, müssen Sie die GNU GPL v3-Lizenz verwenden
- Sie müssen den Quellcode öffentlich machen
- Sie müssen den ursprünglichen Autor und Änderungen angeben
- Es wird keine Garantie gewährt