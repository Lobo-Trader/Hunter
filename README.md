# Hunter Handelssystem – Systembeschreibung

**Hunter ist ein trendfolgendes Multi-Timeframe-Handelssystem für den Forexmarkt.**  
Das System wurde entwickelt, um stabile Trends über mehrere Zeitebenen zu erkennen und nur während optimaler Trendphasen in den Markt einzusteigen. Dadurch werden Fehlsignale reduziert und die Wahrscheinlichkeit für erfolgreiche Trades erhöht.

---

## Grundprinzip

Hunter analysiert vier Zeitebenen (M1, M5, M15, M30) parallel und verwendet folgende Komponenten:

### 1. Trendfilter (EMA50)
- In jedem Timeframe muss der Kurs oberhalb (Long) bzw. unterhalb (Short) der 50er-EMA liegen.
- Nur wenn alle vier Zeitebenen dieselbe Trendrichtung anzeigen, wird ein Einstiegssignal generiert.

### 2. Trendstärke – Chop Zone
- In jedem Timeframe prüft der Chop Zone Indikator anhand des Winkels der EMA34, wie ausgeprägt der Trend ist.
- Es werden neun Farbstufen unterschieden:
  - **Long:** Türkis, Dark Green, Pale Green, Lime (alle als „blau“ für Long)
  - **Short:** Dark Red, Rot, Orange, Light Orange (alle als „rot“ für Short)
  - **Neutral:** Gelb (Standard: In M1 kein Trade bei Gelb; in den höheren TFs optional erlaubt)
- Die Farblogik ist individuell konfigurierbar.

### 3. Trend Momentum (TM/MagicTrend)
- Im Preischart zeigt die MagicTrend-Linie (basierend auf CCI und ATR) das aktuelle Trendmomentum an:
  - **Blau:** CCI >= 0 (Long-Momentum)
  - **Rot:** CCI < 0 (Short-Momentum)
- Nur wenn in allen Timeframes das Momentum in Trendrichtung zeigt, wird ein Einstieg erwogen.

---

## Entry- und Exit-Regeln

### Einstieg (Entry)
- **Long:**  
  - Kurs in allen TFs > EMA50  
  - Chop Zone in allen TFs blau (kein Gelb im M1)  
  - MagicTrend in allen TFs blau
- **Short:**  
  - Kurs in allen TFs < EMA50  
  - Chop Zone in allen TFs rot (kein Gelb im M1)  
  - MagicTrend in allen TFs rot
- Das Einstiegssignal wird im M1 ausgelöst, sobald alle Bedingungen erfüllt sind.

### Stop Loss (SL)
- **Long:** SL wird unter das letzte lokale Tief (Kerze) im M1 gesetzt.
- **Short:** SL wird über das letzte lokale Hoch (Kerze) im M1 gesetzt.

### Exit
- Der Trade wird beendet, wenn das TM-Signal (MagicTrend) im M1 auf die Gegenseite wechselt.

---

## Flexibilität und Erweiterbarkeit
- Farbdefinitionen der Chop Zone sowie das Handling von Gelb sind individuell anpassbar.
- System ist modular für weitere Märkte und Plattformen (z.B. MT5, Python) angelegt.
- Erweiterung auf Listen-Alerts für alle 28 FX-Paare ist vorgesehen.

---

## Ziel des Systems

Hunter filtert erfolgreich Seitwärtsphasen heraus und steigt nur während klarer, stabiler Trends in den Markt ein. Durch die gleichzeitige Prüfung mehrerer Zeitebenen werden Fehlsignale reduziert und der Einstieg auf die vielversprechendsten Trendbewegungen fokussiert.

---
