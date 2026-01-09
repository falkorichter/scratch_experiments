# Scratch Projekt Analyse: "My First Cloud Variable"

## Überblick
Dieses Scratch-Projekt ist ein Klick-Zähler-Spiel, das die Verwendung von Cloud-Variablen und interaktiven Sprite-Verhaltensweisen demonstriert. Das Projekt besteht aus drei Hauptkomponenten: einer Bühne, einem "reak"-Sprite und einem "Block-B"-Sprite.

## Projektkomponenten

### 1. Bühne
Die Bühne dient als Hintergrund und Variablen-Container für das Projekt.

**Variablen:**
- `☁ click_counter` (Cloud-Variable): Zählt die Gesamtanzahl der Klicks (Cloud-Variable - Wert bleibt sitzungsübergreifend erhalten)
- `sprunghöhe`: Sprunghöhen-Variable (initialisiert mit "0")
- `Time Pressed`: Verfolgt die gedrückte Zeit (initialisiert mit "0")
- `vorwärts`: Vorwärtsbewegungsvariable (initialisiert mit 0)
- `bad_behavior`: Verhaltens-Umschaltervariable (initialisiert mit 0)

**Nachrichten:**
- `behavior changed`: Ereignis, das ausgelöst wird, wenn sich der Verhaltenszustand ändert

**Assets:**
- Hintergrund: "Hintergrund1" (SVG-Format)
- Sound: "Plopp" (WAV-Format)

### 2. Sprite: "reak"
Dieses Sprite implementiert zwei parallele Klick-Tracking-Verhaltensweisen.

#### Skript 1: Klick-Erkennung mit Bad-Behavior-Prüfung
**Auslöser:** Wenn die grüne Flagge angeklickt wird

**Logik:**
```
Wiederhole fortlaufend:
  Falls (Maus ist gedrückt UND NICHT bad_behavior gleich 0):
    Wiederhole bis (NICHT Maus ist gedrückt):
      Warte 0.01 Sekunden
    Gehe zu Zufallsposition
    Ändere ☁ click_counter um 1
```

**Verhalten:** Wenn die Flagge angeklickt wird, prüft dieses Skript kontinuierlich, ob die Maus gedrückt ist UND die bad_behavior-Variable NICHT gleich 0 ist (was bedeutet, dass schlechtes Verhalten aktiv ist). Wenn beide Bedingungen erfüllt sind, wartet es, bis die Maus losgelassen wird, bewegt dann das Sprite zu einer Zufallsposition und erhöht den Cloud-Klick-Zähler.

#### Skript 2: Klick-Erkennung mit Good-Behavior-Prüfung
**Auslöser:** Wenn die grüne Flagge angeklickt wird

**Logik:**
```
Wiederhole fortlaufend:
  Falls (Maus ist gedrückt UND bad_behavior gleich 0):
    Gehe zu Zufallsposition
    Ändere ☁ click_counter um 1
```

**Verhalten:** Dieses Skript läuft parallel zu Skript 1. Es prüft, ob die Maus gedrückt ist UND bad_behavior gleich 0 ist (Good-Behavior-Modus). Wenn diese Bedingungen erfüllt sind, bewegt es sich sofort zu einer Zufallsposition und erhöht den Zähler.

**Hauptunterschied:** Das erste Skript wartet auf das Loslassen der Maus, bevor es reagiert (wenn bad_behavior aktiv ist), während das zweite Skript sofort reagiert (wenn gutes Verhalten aktiv ist).

**Assets:**
- Kostüm 1: "Kostüm1" (SVG-Format, katzenähnliches Sprite)
- Kostüm 2: "Kostüm2" (SVG-Format, alternative Katzenpose)
- Sound: "Meow" (WAV-Format)

### 3. Sprite: "Block-B"
Dieses Sprite fungiert als Umschaltknopf zur Steuerung des Verhaltensmodus.

#### Skript 1: Bad Behavior umschalten
**Auslöser:** Wenn dieses Sprite angeklickt wird

**Logik:**
```
Ändere bad_behavior um 1
Setze bad_behavior auf (bad_behavior modulo 2)
Sende Nachricht "behavior changed"
```

**Verhalten:** Beim Anklicken erhöht dies die bad_behavior-Variable und wendet dann Modulo 2 an, wodurch effektiv zwischen 0 und 1 umgeschaltet wird. Nach dem Umschalten sendet es ein "behavior changed"-Ereignis.

#### Skript 2: Verhalten initialisieren
**Auslöser:** Wenn die grüne Flagge angeklickt wird

**Logik:**
```
Setze bad_behavior auf 0
```

**Verhalten:** Initialisiert die bad_behavior-Variable auf 0 (Good-Behavior-Modus), wenn das Projekt startet.

#### Skript 3: Visuelles Feedback
**Auslöser:** Wenn die Nachricht "behavior changed" empfangen wird

**Logik:**
```
Falls bad_behavior gleich 0:
  Wechsle zu Kostüm "bad"
Sonst:
  Wechsle zu Kostüm "good"
```

**Verhalten:** Ändert das Aussehen des Sprites basierend auf dem Verhaltenszustand. Wenn bad_behavior gleich 0 ist, zeigt das Sprite das "bad"-Kostüm; wenn bad_behavior gleich 1 ist, zeigt es das "good"-Kostüm. Dies erzeugt eine invertierte visuelle Anzeige, bei der das aktuelle Zustandslabel darstellt, VON welchem Zustand durch Anklicken des Blocks gewechselt wird, nicht den aktuell aktiven Modus.

**Assets:**
- Kostüm 1: "bad" (SVG-Format, rot/negatives Aussehen)
- Kostüm 2: "good" (SVG-Format, grün/positives Aussehen)
- Sound: "meow" (WAV-Format)

## Spielmechanik

### Wie das Spiel funktioniert:
1. **Initialisierung**: Wenn die grüne Flagge angeklickt wird, wird bad_behavior auf 0 gesetzt (Good-Modus)
2. **Good-Behavior-Modus (bad_behavior = 0)**:
   - Klicken Sie irgendwo, um das "reak"-Sprite zu einer Zufallsposition springen zu lassen
   - Jeder Klick erhöht sofort den Cloud-Zähler
   - Das "Block-B"-Sprite zeigt das "bad"-Kostüm (obwohl es sich im Good-Modus befindet)

3. **Bad-Behavior-Modus (bad_behavior = 1)**:
   - Klicken und halten Sie die Maustaste
   - Das "reak"-Sprite wartet, bis Sie die Maus loslassen
   - Erst dann springt es zu einer Zufallsposition und erhöht den Zähler
   - Das "Block-B"-Sprite zeigt das "good"-Kostüm (obwohl es sich im Bad-Modus befindet)

4. **Modi umschalten**: Klicken Sie auf das "Block-B"-Sprite, um zwischen den Modi umzuschalten

### Cloud-Variable-Funktion:
Die `☁ click_counter`-Variable ist eine Cloud-Variable (angezeigt durch das ☁-Symbol), was bedeutet, dass sie über alle Instanzen des Projekts hinweg geteilt werden kann, wenn es auf scratch.mit.edu gehostet wird. Dies ermöglicht es mehreren Benutzern, zum selben Zähler beizutragen.

## Technische Details

### Scratch-Version:
- Format: Scratch 3.0.0
- VM-Version: 12.1.3
- Erstellt/Bearbeitet in: Chrome-Browser auf macOS

### Verwendete Programmiermuster:
1. **Endlosschleifen**: Kontinuierliche Überwachung der Mauseingabe
2. **Bedingte Logik**: Unterschiedliche Verhaltensweisen basierend auf Zustandsvariablen
3. **Ereignis-Broadcasting**: Sprite-übergreifende Kommunikation
4. **Zustandsverwaltung**: Verwendung der bad_behavior-Variable zur Steuerung des Spielmodus
5. **Modulo-Operation**: Zum Umschalten zwischen zwei Zuständen (0 und 1)

### Interessante Code-Elemente:
- **Parallele Ausführung**: Zwei separate Skripte auf dem "reak"-Sprite behandeln dasselbe Klick-Ereignis unterschiedlich, basierend auf dem Verhaltenszustand
- **Debouncing**: Skript 1 implementiert eine Form von Debouncing, indem es auf das Loslassen der Maus wartet
- **Zufällige Positionierung**: Macht das Spiel herausfordernder, indem das Ziel bewegt wird

## Zweck
Dieses Projekt scheint ein pädagogisches Experiment zu sein, das demonstriert:
- Verwendung von Cloud-Variablen in Scratch
- Zustandsbasiertes Verhaltensumschalten
- Ereignis-Broadcasting zwischen Sprites
- Mauseingabeverarbeitung mit verschiedenen Antwortmustern
- Visuelles Feedback für Zustandsänderungen
