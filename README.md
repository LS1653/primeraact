Nombre del equipo: Equipo alfa buena maravilla escuadrón lobo
Integrante/Mienbros/Aliados: -Esteban Puerta Mejía
                             -Juan Diego Maldonado Tamayo
ID(respectivo):-000509157
               -000513095 
```
void setup() {
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
  static uint32_t previousTime = 0;
  static bool ledState = true;

  uint32_t currentTime = millis();

  if( (currentTime - previousTime) > 500){
    previousTime = currentTime;
    ledState = !ledState;
    digitalWrite(LED_BUILTIN, ledState);
  }
}
```
Al modificar el 100 a 500 lo que sucedia era que el led de la placa del pi pico se encendia y se apagaba en intervalos de tiempo más largos, osea tardaba más tiempo en ensenderse y apagarse, esto debio a que el programa dice que si la resta entre el tiempo transcurrido y el tiempo previo era mayor a 500 se encienda y apaga el led, al entender esto nos damos cuenta que el 100 y el 500 funcionan como el intervalo de tiempo en el que debe prenderse el led, por lo tanto entre mayor sea ese intervalo mayor tiempo se demorara en encenderse y apagrase el led.