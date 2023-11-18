---
title:  "Transactional"
date:   2023-11-16 20:29:12
author: nosov-ao
---


В Spring, уровни транзакционной области определяются с использованием атрибута propagation аннотации @Transactional. Этот атрибут определяет, как будет взаимодействовать текущая транзакция с уже существующей транзакцией (если таковая имеется) при вызове метода, помеченного как транзакционный.

Вот некоторые из уровней propagation:

- REQUIRED (ПО УМОЛЧАНИЮ):
Если текущая транзакция существует, метод будет выполняться в ее контексте.
Если текущей транзакции нет, то будет создана новая.

- SUPPORTS:
Если текущая транзакция существует, метод будет выполняться в ее контексте.
Если текущей транзакции нет, метод будет выполняться без транзакции.

- MANDATORY:
Требует, чтобы текущая транзакция существовала. В противном случае, возникнет исключение.

- REQUIRES_NEW:
Метод будет выполнен в новой транзакции, если текущая существует, она будет приостановлена.

- NOT_SUPPORTED:
Метод будет выполняться без транзакции.
Если текущая транзакция существует, она будет приостановлена.

- NEVER:
Требует отсутствия текущей транзакции. Если текущая транзакция существует, возникнет исключение.

NESTED:
Метод будет выполнен вложенной транзакции, если текущая транзакция существует. В противном случае, будет создана новая транзакция.

Различия между REQUIRED и REQUIRES_NEW в том, что REQUIRED использует текущую транзакцию, если она существует, в то время как REQUIRES_NEW создает новую транзакцию, приостанавливая текущую.