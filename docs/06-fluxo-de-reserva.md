# Fluxo de reserva

1. O usuário escolhe um assento disponível.
2. O booking-service trava o assento no Redis com prazo de expiração definido.
3. Se a compra for concluída, a reserva é gravada no banco.
4. Se o prazo expirar sem conclusão, o lock é liberado e o assento volta a ficar disponível.