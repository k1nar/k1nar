rom aiogram import Bot, Dispatcher, executor, types
import python_weather

#bot
bot = Bot(token='Ваш токен')
dp = Dispatcher(bot)
client = python_weather.Client(format=python_weather.IMPERIAL, locale='ru-Ru')

@dp.message_handler()
async def echo(message: types.Message):
    weather = await client.find(message.text)
    celsius = round((weather.current.temperature - 32) / 1.8)

    resp_q = weather.location_name + '\n'
    resp_q += f'Текущая температура: {celsius}°\n'
    resp_q += f'Состояние погоды: {weather.current.sky_text}'

    if celsius <= 10:
        resp_q += '\nНа улице прохладненько! Одевайся теплее!'
    else:
        resp_q += '\nНа улице тепло! Одевайся полегче!'


    await message.answer(resp_q)

if __name__ == '__main__':
    executor.start_polling(dp, skip_updates=True)
