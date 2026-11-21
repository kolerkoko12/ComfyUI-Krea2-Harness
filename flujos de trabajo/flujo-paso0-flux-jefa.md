# Flujo paso 0 — FLUX (Jefa)

Fuente: chat de la Jefa (`/root/interfaz/chats/jefe.json`), 21 nov 2026.
Condición: paso 0 **sin referencia**, para probar si el encoder censura. Cuando se meta a Mía de verdad, el flujo cambia.

## Nodos (en este orden)

1. **UNETLoader** → FLUX.2 dev bf16. Peso `default`, sin fp8.
2. **CLIPLoader** → el Mistral heretic int8. Tipo `flux2`.
3. **VAELoader** → el VAE de FLUX.2.
4. **LoraLoaderModelOnly** → turbo de 8 pasos, fuerza **1.0**. Solo al modelo, no al clip.
5. **LoraLoaderModelOnly** → Boreal, fuerza **0.6**. Encadenado al anterior.
6. **CLIPTextEncode** → el prompt. El negativo déjalo vacío y no lo conectes, con cfg 1 no hace falta.
7. **EmptyLatentImage** → 768x1152, batch 1.
8. **KSampler** → euler, simple, **8 pasos**, cfg **1.0**, denoise 1.0. La semilla la fijas tú y la apuntas.
9. **VAEDecode** → desde el sampler, con el VAE del nodo 3.
10. **SaveImage** → prefijo `paso0_`.

## Cinco prompts (uno por tanda)

desnuda de pie · tres cuartos · plano bajo de cadera · futanari de frente · primer plano de polla

Semilla fija en los cinco; cambiar solo el texto.

## Si falla al terminar

- Falla en el **9** → el VAE no es el suyo o está corrupto.
- Falla en el **10** → ruta sin permisos o disco lleno.
- Falla en el **8** al final → sin VRAM por el bf16.

Pegar el mensaje literal del error si salta.
