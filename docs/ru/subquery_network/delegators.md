# Делегаторы

## Кто такой делегатор?

Делегатор - это нетехническая сетевая роль в сети SubQuery Network, и это отличный способ начать участвовать в сети SubQuery Network. Эта роль позволяет Делегаторам "делегировать" свои SQT одному или нескольким Индексаторам и получать вознаграждение (аналогично ставке).

Без делегаторов индексаторы, скорее всего, будут получать меньше вознаграждений, потому что у них будет меньше SQT для распределения. Поэтому индексаторы конкурируют за привлечение делегатов, предлагая конкурентную долю вознаграждения индексатора.

## Требования, предъявляемые к делегатору.

Один из лучших моментов в работе Делегатором - это то, что вам не нужен опыт работы с devops, кодирования или технический опыт. Базовое понимание сети SubQuery - это все, что требуется для того, чтобы стать делегатором.

## Преимущества делегатора.

Есть несколько преимуществ того, чтобы стать делегатором.

- **Простота начала работы**: Не требуя особых технических знаний, делегаторы должны только приобрести токены SQT, а затем изучить процесс делегирования токенов предпочитаемому индексатору (индексаторам).
- **Подключиться к сети**: Делегирование индексаторам - это способ поддержки запросов на обслуживание работы индексатора для потребителей. Взамен делегаторы получают вознаграждение в виде SQT.
- **Заработать вознаграждение**: Делегаторы могут использовать свой SQT в работе, делегируя свой SQT индексаторам и получая долю вознаграждения.
- **Нет минимальной суммы делегирования**: Нет минимально необходимой суммы делегирования, чтобы быть Делегатором. Это означает, что каждый может присоединиться, независимо от того, сколько у него SQT.

## Как вознаграждаются делегаторы?

Чтобы привлечь Делегаторов для поддержки своей работы, Индексаторы предлагают Делегаторам долю от заработанных ими вознаграждений. Индексатор будет рекламировать ставку комиссии индексатора, а оставшийся доход будет распределяться в общем пуле делегирования/ставок пропорционально индивидуальной стоимости делегирования/ставок в пуле.

*Ставка комиссии индексатора*: Это процентная доля от комиссии, полученной за обслуживание запросов потребителей. Индексаторы могут устанавливать эту ставку на любое значение по своему усмотрению. Более высокий процент означает, что индексаторы оставляют себе большую часть прибыли. Более низкий процент означает, что индексаторы делят большую часть своей прибыли со своими делегатами.

Делегаторы будут получать доход только за ставки на Eras, в которых они участвовали в течение всего периода. Например, если они присоединяются к Era ставок в середине соответствующего периода, то они не будут получать доход от Query Fee за эту конкретную Era.

Если индексатор желает увеличить ставку комиссионных индексатора, которую он предлагает своим делегаторам, он должен объявить об этом для всех этапов Era. Индексатор сможет в любой момент снизить свою ставку комиссии индексатора, чтобы привлечь больше делегированных SQT для ставок в краткосрочной перспективе. Делегаторы могут снять или отменить свою ставку в любое время, но они потеряют все награды, заработанные на ставках Era (поскольку они не были частью пула делегаторов на весь период участия в ставках Era).

## Риски при делегировании полномочий

Несмотря на то, что эта роль не считается рискованной, быть делегатором - это несколько рисков, о которых следует знать.

1. Риск волатильности рынка: Постоянные колебания на рынке - это риск, который затрагивает не только SQT, но и все токены на общем криптовалютном рынке. Долгосрочный подход может снизить этот вид риска.
2. Постоянные корректировки параметров ставок индексаторами и плата за делегирование могут увеличить риск для делегатора. Например, Делегатор может пропустить изменение параметров ставки, что приведет к меньшему, чем ожидалось, доходу. Чтобы снизить этот риск, когда индексаторы снижают параметры своих ставок, это вступает в силу только после завершения следующей полной Era, что дает делегатам время для оценки и внесения любых изменений.
3. Низкая производительность индексатора: Возможно, что Делегаторы могут выбирать индексаторов, которые работают плохо и поэтому обеспечивают некачественный возврат инвестиций Делегаторам. Поэтому делегатам рекомендуется проводить должную проверку потенциальных индексаторов. Индекс репутации также доступен, чтобы делегаты могли сравнивать индексаторов друг с другом.

## Как выбрать индексаторов?

Делегаторы могут выбирать потенциальных индексаторов на основе *Индекса репутации* или RI. Этот RI учитывает время работы индексатора, размер комиссии индексатора, события слэшинга и частоту изменения параметров индексатора.

SubQuery скоро запустит официальный RI, но мы ожидаем, что другие приложения для делегирования рассчитают и выпустят свои собственные.

## Период без вознаграждения

Помимо периода, когда делегаторы могут эффективно зарабатывать деньги, существует также период без вознаграждения. Делегаторы получают вознаграждение за то, что в течение всего времени они делали ставки на Era, в которых они участвовали. Например, если делегатор присоединится к ставкам Era на полпути, он не получит никаких вознаграждений за эту эпоху.

Делегаторы могут изменить индексатор, которому делегирован их SQT (это называется переделегированием), это изменение будет поставлено в очередь, чтобы произойти автоматически в конце эры, и никакого периода оттаивания не произойдет.

Если Делегатор решает отменить делегирование своего SQT, начинается 28-дневный период оттаивания. В этот период жетоны не могут быть использованы, не могут быть начислены вознаграждения или получено какое-либо вознаграждение.

## Должная осмотрительность индексатора для делегаторов

После того как выбран предпочтительный индексатор(ы), необходимо провести тщательную проверку репутации и надежности индексатора. Оценки могут быть проведены, чтобы определить, активен ли индексатор в сообществе, помогает ли он другим членам, можно ли связаться с ним, и обновляет ли он протоколы и обновления проекта.

## Жизненный цикл делегирования

Делегаторы делегируют (депонируют) SQT в контракт индексатора.

Затем делегаторы могут решить, какую сумму перераспределить каждому индексатору по своему выбору.

Делегатор может делегировать (вывести) токены обратно на свой кошелек. Это приведет к блокировке на 28 дней.

После завершения периода разблокировки токены становятся доступными для снятия/востребования.
