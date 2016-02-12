---
layout: post
title:  "Dispositif de phototaxie"
date:   2016-02-12 09:44:07
categories: arduino
---


Permettant à un véhicule de se diriger vers la lumière

# Exercices Arduino à consulter
- 05 Indicateur d'humeur
- 06 Thérémine


# Principe
1. Comparer la valeur d'éclairement fournie par deux photo-résistances orientées vers l'avant à 45 deg. de part et d'autre de l'axe de progression.
2. Faire agir le servo-moteur en direction de l'éclairement le plus grand.

# Schéma
![schema](https://cloud.githubusercontent.com/assets/34697/13004431/be8095ec-d17b-11e5-86f4-61ed0317e9c8.png)

# Matériel
- Photo-résistances		2u
- Résistances 10KΩ		2u
- Servo-moteur		    1u
- Condensateur 100µF	1u


# Code

{% highlight c %}
// Direction d'un mobile vers une source de lumière

// Modules à consulter :
// 06 Thérémine pour la mesure de la lumière
// 05 Indicateur d'humeur pour le câblage
// Problèmes à régler: néant

// Déclaration des variables globales
int valEcl_G; // Valeur instantanée d'éclairement du coté gauche
int valEcl_D; // Valeur instantanée d'éclairement du coté droit
int difference; // Différence d'éclairement
int angle; // Angle de rotation du servo-moteur (0<angle<179)

#include  <Servo.h> // Importation de la bibliothèque Servo
Servo servoDirection; // Déclaration du servo-moteur de direction

void setup() {
  Serial.begin(9600);
  servoDirection.attach(6); // La sortie pseudo-analogique 6 pilote le servo de direction
}

void loop() {
  valEcl_G = analogRead(A0); // Lire la valeur délivrée par la photorésistance de gauche
  valEcl_D = analogRead(A1); // Lire la valeur délivrée par la photorésistance de droite
  difference = (valEcl_G - valEcl_D);
  angle = ((difference + 1024)/12); // Conversion d'une valeur comprise entre -1024 et +1024 en une valeur comprise entre 0 et 179

  servoDirection.write(angle); // Commande le servo-moteur de direction

  Serial.print("Gauche : ");
  Serial.print(valEcl_G);
  Serial.print("     Droite : ");
  Serial.print(valEcl_D);
  Serial.print("     Difference : ");
  Serial.print(difference);
  Serial.print("     Angle : ");
  Serial.println(angle);

  delay(100);
}
{% endhighlight %}
