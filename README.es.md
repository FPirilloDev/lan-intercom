# 🖥️🔌🖥️ Intercomunicador LAN — Puesto de trabajo con dos PCs

**Una configuración casera de red que hace que dos PCs Windows independientes se
comporten en parte como un solo puesto de trabajo:** un mismo mouse y teclado en
las dos pantallas, portapapeles compartido, transferencia de archivos por arrastre,
y una carpeta de red compartida por la LAN.

> 🇬🇧 English version → [README.md](README.md)
>
> ℹ️ **Es un proyecto de documentación, no de software** — sin código, base de datos
> ni despliegue. Documenta una configuración real, hecha y verificada a mano.

<!-- ADD:badges -->
`Windows 11` · `PowerToys — Mouse sin bordes` · `SMB (uso compartido)` ·
`Switch Gigabit (Tenda SG105)` · `LAN`

---

## Qué hace

- **Mouse y teclado compartidos.** El cursor cruza de una pantalla a la otra al
  llegar al borde, y el teclado sigue al cursor. Cualquiera de las dos máquinas
  puede tomar el control con su propio periférico físico.
- **Portapapeles compartido.** Texto copiado en una máquina se pega en la otra.
- **Transferencia de archivos** por arrastre (archivos sueltos) y una **carpeta de
  red compartida** (SMB) montada como unidad de red.
- **Layout tipo multi-monitor.** Disposición física configurada para que el cursor
  respete los bordes en vez de dar la vuelta.

## Cómo funciona

```
Módem/router del proveedor ──► Tenda SG105 (switch Gigabit) ──► PC de escritorio
                                                       └─────► Notebook
```

- **Topología:** el switch cuelga del router del proveedor, así ambas PCs comparten
  la misma subred — conservando internet y DHCP y ganando visibilidad mutua.
- **Software:** Microsoft **PowerToys** (módulo Mouse sin bordes) en las dos
  máquinas, emparejadas con una clave de seguridad; **SMB nativo de Windows** para la
  carpeta compartida. La comunicación es puramente local — no requiere internet. Las
  máquinas se resuelven por nombre de equipo, así que reconecta aunque el router
  asigne otra IP.

Más detalle: [`docs/topology.md`](docs/topology.md)

## Un problema real resuelto (troubleshooting)

La notebook se congelaba (barra de tareas y cambio de programas sin responder).
Causa raíz, **diagnosticada por el autor**: ocurría al abrir una terminal/ventana
**como administrador** — el aislamiento de privilegios de UAC impide que un proceso
sin privilegios inyecte input en una ventana elevada, congelando los periféricos
compartidos. El camino del arreglo se deduce de ese diagnóstico (ejecutar la
herramienta de emparejamiento con el mismo nivel de privilegio / evitar ventanas
elevadas durante el control compartido).

## Estado

Funcional y en uso. El mouse/teclado compartido, el portapapeles y el arrastre
andan. La carpeta compartida por SMB quedó a medio configurar (un pedido de
credenciales pendiente). <!-- ADD:estado -->

## Decisiones clave

- **No comprar un switch caro.** Se evaluó un switch PoE administrable y se descartó
  — el PoE y la administración eran irrelevantes; un Gigabit no administrado Tenda
  SG105 alcanzaba.
- **Switch colgado del router, no aislado.** Conserva internet + DHCP (sin IPs
  manuales).
- **Mouse sin bordes sobre Input Leap/Barrier.** Ambas máquinas son Windows, así que
  la herramienta integrada de Microsoft fue más simple y permite arrastrar archivos.
- **Expectativa aclarada:** no se pueden mover *ventanas* entre dos sistemas
  operativos independientes — solo se comparten periféricos, portapapeles y archivos.

## Autoría

**Dirigido y ejecutado por el autor; asistencia técnica de la IA.** El autor definió
el objetivo, ejecutó cada cambio en sus máquinas, verificó cada paso y **diagnosticó
por su cuenta el problema del congelamiento** (la IA no lo había identificado). La IA
aportó el conocimiento técnico y las instrucciones paso a paso.

## Licencia

[MIT](LICENSE) · _Poné como titular tu nombre/usuario de GitHub._
