---
title:  "Race condition"
date:   2022-11-10 21:29:12
author: nosov
---

# 	Race condition (гонка состояний) - это ситуация в многозадачной среде, когда порядок выполнения операций зависит от времени выполнения, что может привести к непредсказуемому поведению программы

###   Способы борьбы с race condition: 

 - Оптимистическая блокировка (Optimistic Locking): Это подход, при котором система предполагает, что конфликты между параллельными операциями будут редкими. Вместо того чтобы блокировать ресурс на протяжении всей операции, поток или процесс начинает считать, что операция выполнится успешно. После завершения операции система проверяет, произошли ли конфликты. Если конфликты не обнаружены, изменения сохраняются. В случае конфликта система должна решить, как обрабатывать ситуацию, например, повторить операцию.

 - Пессимистическая блокировка (Pessimistic Locking) - это стратегия управления параллельным доступом к общим ресурсам, при которой система предполагает возможность конфликтов и предпринимает активные меры для предотвращения их возникновения. Она основывается на идее "пессимистичного" предположения о возможности конфликтов, и поэтому при ее использовании ресурсы обычно блокируются, чтобы предотвратить другим потокам или процессам доступ к ним.

 - Изоляция (Isolation): Это концепция, связанная с тем, насколько операции в различных потоках или процессах могут быть изолированы друг от друга. Изоляция контролирует видимость изменений, внесенных одной операцией, другим операциям в системе. В многозадачных и многопользовательских системах изоляция играет важную роль, чтобы предотвратить конфликты и обеспечить непрерывную и надежную работу приложений.


Примером пессимистической блокировки может служить использование synchronized в Java
Примером оптимистической блокировки может служить использование AtomicInteger в Java


|       issue        | isolation level |
|:------------------:|:--------------:|
|     Dirty Read     | Read uncommited |
| Non repitable read | Read commited  |
|    Phantom read    | Repetable read |
|                    |  Serializable  |

### Феномены race condition, которые могут возникнуть при реализации оптимистической блокировки, включают:
 - Lost Update (Потерянное обновление): Если два или более потока читают данные, модифицируют их, а затем пытаются записать обновленные данные, только одно из обновлений будет сохранено, и остальные будут потеряны.

 - Dirty Read (Грязное чтение): Если один поток читает данные, а затем другой поток модифицирует эти данные, первый поток может прочитать "грязные" (неправильные) данные, которые еще не были подтверждены.

 - Inconsistent Retrieval (Непоследовательное извлечение): Если другой поток изменяет данные, которые были прочитаны первым потоком, прежде чем первый поток успеет записать свои изменения, то конечное состояние данных станет несогласованным.

 - Write Skew (Смешанная запись): Несколько потоков могут одновременно читать данные и попытаться их изменить, что приведет к несогласованным изменениям.

### Феномены race condition при реализации пессимистической блокировки включают:
 - Deadlocks (Взаимные блокировки): Это явление, при котором два или более потока удерживают ресурсы и ждут друг друга, чтобы освободить другие ресурсы. Это может привести к тому, что программы зависают, так как они ожидают освобождения ресурсов, которые никогда не будут освобождены.

 - Priority Inversion (Инверсия приоритета): Если поток с более низким приоритетом удерживает блокировку, к которой поток с более высоким приоритетом хочет получить доступ, это может привести к инверсии приоритета и, как следствие, к непредсказуемой задержке выполнения высокоприоритетного потока.

 - Lock Contention (Состязание за блокировки): Если много потоков пытаются получить доступ к общему ресурсу, это может вызвать состязание за блокировки, что приводит к задержкам и ухудшению производительности.

 - Ждущие потоки (Blocking Threads): Если поток удерживает блокировку и не может выполнить операцию, другие потоки, которые пытаются получить доступ к тому же ресурсу, должны ждать.

 - Starvation (Голодание): В случае долговременного удержания блокировки одним из потоков другие потоки могут испытывать голодание, не имея доступа к ресурсу.

### Предотвращение взаимной блокировки (deadlock):
 - Установка порядка блокировки (Lock Ordering): Один из подходов к предотвращению взаимной блокировки - это установка строгого порядка блокировок и требование, чтобы все потоки следовали этому порядку при захвате блокировок. Это снижает вероятность взаимной блокировки, так как потоки всегда захватывают блокировки в одном и том же порядке.

 - Таймауты блокировок (Lock Timeout): Установка временных ограничений на удержание блокировки может помочь предотвратить взаимную блокировку. Если поток не может захватить блокировку в течение определенного времени, он может отпустить уже удерживаемые блокировки и попытаться снова.

 - Иерархическая блокировка (Hierarchical Locking): Разделение блокировок на уровни иерархии и требование, чтобы потоки могли блокировать только блокировки более низкого уровня, чем те, которые они уже удерживают, может снизить вероятность взаимной блокировки.

 - Использование неблокирующих алгоритмов (Non-blocking Algorithms): Использование алгоритмов, которые не требуют блокировок, таких как атомарные операции или структуры данных с поддержкой нескольких потоков, может полностью избежать проблемы взаимной блокировки.

 - Мониторы и условные переменные (Monitors and Condition Variables): Использование мониторов и условных переменных может помочь в предотвращении взаимной блокировки, предоставляя механизмы для ожидания событий и освобождения ресурсов по требованию.

 - Использование атомарных операций (Atomic Operations): В языках программирования с поддержкой атомарных операций можно использовать операции, которые гарантируют, что выполнение операции не будет прервано другим потоком.

 - Мониторинг и анализ блокировок (Lock Monitoring and Analysis): Использование инструментов мониторинга и анализа для выявления и отслеживания взаимных блокировок может помочь в предотвращении проблем на ранних стадиях разработки.