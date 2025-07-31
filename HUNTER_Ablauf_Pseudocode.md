# Ablauf & Pseudocode für Hunter_V1.00

## Übersicht

Der folgende Ablauf beschreibt die Signal-Logik für den Hunter-Indikator (Version 1.00) in TradingView/Pine Script.  
Ziel: Einstiegssignale (Long/Short) mit Visualisierung, Prüfung aller Bedingungen und ausführlichem Debugging.

---

## 1. Initialisierung und Inputs

- Definiere Timeframes: M1, M5, M15, M30
- Hole für alle TF:
    - Preis (close)
    - EMA50
    - MagicTrend (TM)
    - Chop Zone Farbe
- Input-Parameter:
    - Farblogik Chop Zone (was ist „blau“, „rot“, „gelb“)
    - Gelb-Handling: im M1 nie, in höheren TF optional
    - Debug-Modus (an/aus)
    - Visualisierung: Pfeil, Linie, Label (wahlweise)

---

## 2. Vorberechnung je Timeframe

Für jeden TF:
- Berechne EMA50
- Berechne MagicTrend (CCI, ATR, siehe TM-Code)
- Berechne Chop Zone Farbe (siehe Code)
- Speichere alle Werte für Haupt-TF (M1, "Basis") und Kontroll-TFs

---

## 3. Hauptlogik – Signalprüfung

### LONG-Bedingung:
- In allen TF:
    - close > EMA50
    - Chop Zone ist „blau“ (laut Farblogik); KEIN „gelb“ im M1
    - MagicTrend ist blau
- Wenn **alle** Bedingungen erfüllt → LONG-SIGNAL

### SHORT-Bedingung:
- In allen TF:
    - close < EMA50
    - Chop Zone ist „rot“ (laut Farblogik); KEIN „gelb“ im M1
    - MagicTrend ist rot
- Wenn **alle** Bedingungen erfüllt → SHORT-SIGNAL

---

## 4. Signalvisualisierung

- Zeichne Pfeil (bzw. Linie/Label) am Signalpunkt im M1-Chart
- Optional: Farbe abhängig von Signalrichtung (Long/Short)
- Debug/Info-Box:
    - Zeigt pro Kerze: alle Einzelbedingungen und deren Status (true/false)
    - Zeigt, warum kein Signal entsteht (z.B. „M15 ChopZone=gelb“, „M5 TM=rot“ etc.)

---

## 5. Stop Loss Berechnung

- Setze SL:
    - Long: unter das letzte Kerzentief im M1
    - Short: über das letzte Kerzenhoch im M1

---

## 6. Exit-Regel (nur Info, keine Chart-Ausführung in V1.00)

- Trade wird beendet, wenn MagicTrend im M1 dreht (z.B. von blau auf rot)

---

## 7. Modulare Erweiterbarkeit

- Alle Parameter als Inputs
- Basis für spätere Erweiterungen: Multi-Pair, Alerts, MT5/Python

---

## Pseudocode (schematisch)

```pseudocode
// Initialisierung
inputs = {ChopZoneFarben, GelbHandling, Debug, VisualStyle}
timeframes = [M1, M5, M15, M30]
for tf in timeframes:
    ema50[tf] = calcEMA50(tf)
    magicTrend[tf] = calcMagicTrend(tf)
    chopZone[tf] = calcChopZone(tf)
    close[tf] = getClose(tf)

// Signalprüfung LONG/SHORT
longSignal = true
shortSignal = true
for tf in timeframes:
    if close[tf] <= ema50[tf] or !isChopZoneBlau(chopZone[tf], tf) or !isMagicTrendBlau(magicTrend[tf]):
        longSignal = false
    if close[tf] >= ema50[tf] or !isChopZoneRot(chopZone[tf], tf) or !isMagicTrendRot(magicTrend[tf]):
        shortSignal = false

// Visualisierung & Debug
if longSignal:
    showArrow("LONG", style=inputs.VisualStyle)
    logDebugInfo("LONG", allConditions)
if shortSignal:
    showArrow("SHORT", style=inputs.VisualStyle)
    logDebugInfo("SHORT", allConditions)
if !longSignal && !shortSignal && inputs.Debug:
    logDebugInfo("NONE", allConditions)

// Stop Loss Info
if longSignal:
    stopLoss = getLastLow(M1)
if shortSignal:
    stopLoss = getLastHigh(M1)
```
---

## Hinweise

- Alle Prüfungen laufen synchron pro Kerze/M1, die Kontroll-TFs werden jeweils auf die aktuelle M1-Kerze projiziert (synced).
- Die tatsächliche Pine-Implementierung nutzt security()-Aufrufe für Multi-TF-Abfragen.
- Ziel: Maximale Nachvollziehbarkeit, saubere Trennung von Signal- und Debug-Logik.

---