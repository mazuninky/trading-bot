# Стратегии

Изначально для бота необходимо задать промежуток времени, через которое он будет принимать решение(частота бота). Одним из ограничений для торговли на фондовом рынке: время работы биржы, поэтому необходимо добавить время работы бота.

Драфт DSL:
```kotlin
val shortBot = strategy(name = "Short bot", frequency = minute(1)) {
    time(hour(9) to hour(18))
    history(day(12))
    onUpdate { update ->
        if(update.price < 5)
            // покупка по рыночной цене
            buy(1)
        else
            sell(all)
    }
}

suspend fun main() {
    tradingBot(Tinkoff, Influx) {
        include(shortBot)
    }.start()
}
```



## Требования к DSL

- Возможность описывать частоту бота
- Возможность описать время работы бота
- Возможность указать стоп условия для бота(к примеру, бот начал в убыток работать)
- Возможность стратегии иметь состояние
- Возможность описывать индикаторы
- Возможность для стратегии записывать данные для длительного хранения
- Возможность описать логику, которая срабатывает в частоту бота
- Возможность описывать через dsl условия для обработки различных событий(к примеру, вызови функцию x, если цена будет больше 10)