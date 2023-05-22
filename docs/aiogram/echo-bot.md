# Создание эхо бота

```python
import logging
from aiogram import Bot, Dispatcher, types

# Устанавливаем уровень логов на INFO, чтобы видеть сообщения о действиях бота
logging.basicConfig(level=logging.INFO)

# Создаем объекты бота и диспетчера
bot = Bot(token='YOUR_TOKEN')
dp = Dispatcher(bot)

# Обработчик команды /start
@dp.message_handler(commands=['start'])
async def send_welcome(message: types.Message):
    await message.reply("Привет! Я эхо-бот. Отправь мне сообщение, и я повторю его.")

# Обработчик всех остальных сообщений
@dp.message_handler()
async def echo_message(message: types.Message):
    await message.answer(message.text)

# Запускаем бота
if __name__ == '__main__':
    dp.start_polling()
```

---

Вам потребуется заменить 'YOUR_TOKEN' на токен вашего бота, который можно получить у `@BotFather` в Telegram. Убедитесь, что у вас установлена библиотека `aiogram` (`pip install aiogram`), прежде чем запускать код.

После запуска этого кода бот будет отвечать на команду `/start` приветственным сообщением. Кроме того, он будет повторять все остальные сообщения, которые вы отправите ему.