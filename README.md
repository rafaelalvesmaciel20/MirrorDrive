# MirrorDrive V2 — transmissão + receiver

Esta versão adiciona o núcleo de vídeo sobre a rede local:

- `MediaProjection` -> `VirtualDisplay` -> `MediaCodec` H.264
- Packetização RTP/UDP simplificada com FU-A
- Descoberta prática por broadcast UDP
- Receiver Android com `MediaCodec` + `SurfaceView`
- O mesmo APK pode ser instalado no celular (Transmitir tela) e em um dispositivo Android receptor (Receiver).

## Teste
1. Instale o APK/projeto em dois Androids na mesma rede Wi-Fi.
2. No aparelho receptor, abra MirrorDrive > Receiver.
3. No celular, abra MirrorDrive > Transmitir tela e autorize a captura.
4. O transmissor envia para broadcast UDP na porta 5000.

## Limitações da V2
- O transporte é uma implementação inicial de RTP/UDP, sem FEC, jitter buffer, criptografia ou controle de congestionamento.
- O receiver atual assume fluxo H.264 Annex-B e resolução inicial 1920x1080; para produção, deve ler SPS/PPS e adaptar a resolução.
- Áudio, USB, pareamento seguro, descoberta robusta e Mirror AI ainda são módulos posteriores.
- Android Auto não é transformado em receptor genérico. Para usar esta arquitetura no carro, o receiver precisa rodar em uma box/central Android compatível.
- Não há bypass de DRM, FLAG_SECURE ou restrições de segurança automotiva.

## V2 commercial-core additions
- Receiver discovery via UDP multicast (`239.255.77.77:47777`) with manual-IP fallback.
- RTP/H.264 sender with FU-A fragmentation, dynamic RTP timestamp from MediaCodec PTS, and unicast transport.
- Receiver RTP reassembly with a small jitter/reordering buffer and Annex-B input to the hardware decoder.
- Foreground media-projection service and explicit receiver target selection.

## Production hardening still required before public release
- TLS/authenticated control channel and pairing keys.
- Network congestion control, packet-loss recovery/FEC, adaptive bitrate and resolution.
- Hardware/SoC compatibility matrix and decoder capability negotiation.
- Audio capture/codec path with per-app capture restrictions respected.
- Crash reporting, telemetry with consent, automated tests, battery/thermal profiling.
- USB transport implementation for selected receiver hardware.
- Store compliance, privacy policy, licensing and security review.
