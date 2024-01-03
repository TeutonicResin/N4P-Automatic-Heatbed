Hello printing enthusiasts,

Did you know that the Elegoo Neptune 4 Pro doesn't just have one, but two heated bed zones? Yes, you heard it right - double the fun, double the warmth! However, Elegoo suggests that we all switch to the included outdated Cura Slicer to make use of this feature.

Well, I, out of conviction, opted for the Orca Slicer and faced the question: How do I get this whole thing up and running?

In my scenario, it was important to me that Klipper decides whether the outer heated bed is necessary or not. After all, who wants to manually intervene when the model leaves the central 120mm x 120mm printing zone? This should happen automatically, without any drama.

Here's my approach:
I determine the model's position with the slicer (Orca), and I pass the internal parameters (first_layer_print_min/first_layer_print_max) to Klipper. These must be appended to the START_PRINT command in the Start G-code. Here's a small example of how it looks:

```
START_PRINT BED_TEMP=[bed_temperature_initial_layer_single] EXTRUDER_TEMP=[nozzle_temperature_initial_layer] AREA_START={first_layer_print_min[0]},{first_layer_print_min[1]} AREA_END={first_layer_print_max[0]},{first_layer_print_max[1]}
```

After that, the Heatbed Macro, which you inserted into the `printer.cfg` beforehand, grabs the parameters and checks whether the print has left the central print bed.

The standard M140 Macro is commented out and replaced with the modified version. This one checks which beds are active because we want to avoid accidental, unwanted "reheating" of the outer bed during the second layer, right?

To top it off, the START_PRINT Macro is adjusted. The heating (M190, M140, etc.) is replaced by this command:

```
HEATBED_AUTO BED_TEMP={params.BED_TEMP|default(60)|float} AREA_START={params.AREA_START|default("0,0")} AREA_END={params.AREA_END|default("0,0")}
```

I replaced my N4P firmware with a standard Klipper version, so your printer may need individual adjustments. Look at the component names and compare them with your system. Another small tip: Elegoo seems to have put a lot of logic into the G-code script in the slicer and less into the START_PRINT Macro in Klipper. So, carefully check where your heated beds come to life.

And now, the disclaimer: These macros are made just for me. I have no idea if they will work for you or if your printer will suddenly catch fire, dance the tango, whatever... So, use them only if you understand them and at your own risk.

Have fun printing!

---

Hallo Druckfreunde,

Wisst ihr, der Elegoo Neptune 4 Pro hat nicht nur ein, sondern gleich zwei Heizbettzonen. Ja, richtig gehört - doppelter Spaß, doppelte Wärme! Aber Elegoo denkt, wir sollten alle auf den mitgelieferten, veralteten Cura-Slicer umsteigen, um das Ganze zu nutzen. 
Nun ja, ich hab mich aus Überzeugung für den Orca-Slicer entschieden und stand vor der Frage: Wie bringe ich das ganze zum Laufen?

In meiner Problemstellung war es mir wichtig, dass Klipper selbst entscheidet, ob das äußere Heizbett notwendig ist oder nicht. Denn wer will schon manuell eingreifen, wenn das Modell die zentrale 120mm x 120mm-Druckzone verlässt? Das sollte automatisch passieren, ganz ohne Drama.

Daher mein Vorgehen:
Die Position des Modells kläre ich mit dem Slicer (Orca), und die internen Parameter (first_layer_print_min/first_layer_print_max) reiche ich Klipper weiter. Die müssen also im Start-GCODE dem START_PRINT-Command angehangen werden. Hier ein kleines Beispiel, wie das aussieht:

```
START_PRINT BED_TEMP=[bed_temperature_initial_layer_single] EXTRUDER_TEMP=[nozzle_temperature_initial_layer] AREA_START={first_layer_print_min[0]},{first_layer_print_min[1]} AREA_END={first_layer_print_max[0]},{first_layer_print_max[1]}
```

Danach schnappt sich das Heatbed-Macro, welches ihr vorher in die `printer.cfg`eingefügt habt, die Parameter und checkt, ob der Druck das zentrale Druckbett verlassen hat.

Das standard M140-Macro wird auskomment und durch die modifizierte Version ersetzt. Diese schaut nach, welche Betten aktiv sind. Denn wir wollen ja ein versehentliches, ungewolltes "Nachheizen" des außeren Bettes beim zweiten Layer vermeiden, oder?

Zum krönenden Abschluss wird das START_PRINT-Macro angepasst. Das Aufheizen (M190, M140, usw.) wird durch diesen Befehl ersetzt:

```
HEATBED_AUTO BED_TEMP={params.BED_TEMP|default(60)|float} AREA_START={params.AREA_START|default("0,0")} AREA_END={params.AREA_END|default("0,0")}
```

Ich hab meine N4P-Firmware gegen eine Standard-Klipper-Version ausgetauscht, also kann es sein, dass euer Drucker individuelle Anpassungen braucht. Schaut euch die Komponentennamen an und vergleicht sie mit eurem System.Noch ein kleiner Tipp: Elegoo hat scheinbar viel Logik ins GCODE-Script im Slicer gesteckt und weniger ins START_PRINT-Macro in Klipper. Checkt also genau, wo eure Heizbetten zum Leben erweckt werden.

Und jetzt der Disclaimer: Diese Macros sind nur für mich gemacht. Keine Ahnung, ob sie bei euch funktionieren oder euer Drucker plötzlich brennt, Tango tanzt, whatever... Also, nutzt sie nur, wenn ihr sie versteht und auf eigene Gefahr.

Viel Spaß beim Drucken!
