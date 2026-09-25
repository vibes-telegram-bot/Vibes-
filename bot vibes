import asyncio
import logging
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from aiogram import Bot, Dispatcher, types, F
from aiogram.filters import Command, StateFilter
from aiogram.fsm.context import FSMContext
from aiogram.fsm.state import State, StatesGroup
from aiogram.fsm.storage.memory import MemoryStorage
from aiogram.types import ReplyKeyboardMarkup, KeyboardButton, ReplyKeyboardRemove

# ===================== НАСТРОЙКИ =====================
BOT_TOKEN = "8972677371:AAGlKiQKvfbrJJN4hoO_sTfuzWvRp8dlVNc"

# Настройки почты (используем одну и ту же для заявок и вопросов)
SMTP_SERVER = "smtp.mail.ru"
SMTP_PORT = 587
SMTP_LOGIN = "kupcoiric@mail.ru"       # отправитель
SMTP_PASSWORD = "280784Hw"  # пароль приложения

# ===================== ИНИЦИАЛИЗАЦИЯ =====================
logging.basicConfig(level=logging.INFO)
storage = MemoryStorage()
bot = Bot(token=BOT_TOKEN)
dp = Dispatcher(storage=storage)

# ===================== КНОПКИ ГЛАВНОГО МЕНЮ =====================
button_who = KeyboardButton(text="👥 Для кого проект?")
button_benefits = KeyboardButton(text="✨ Чем полезен проект?")
button_info = KeyboardButton(text="ℹ️ Участникам")
button_partners = KeyboardButton(text="🤝 Юридическим лицам")
button_video = KeyboardButton(text="▶️ Видео о проекте")
button_events = KeyboardButton(text="📅 Мероприятия")
button_question = KeyboardButton(text="❓ Задать вопрос")
button_apply = KeyboardButton(text="📝 Подать заявку")

main_keyboard = ReplyKeyboardMarkup(
    keyboard=[
        [button_who, button_benefits],
        [button_info, button_partners],
        [button_video, button_events],
        [button_question, button_apply]
    ],
    resize_keyboard=True
)

# ===================== СОСТОЯНИЯ ДЛЯ ЗАЯВКИ =====================
class ApplicationForm(StatesGroup):
    name = State()
    age = State()
    phone = State()
    situation = State()
    documents = State()
    confirm = State()

# ===================== СОСТОЯНИЯ ДЛЯ ВОПРОСА =====================
class QuestionState(StatesGroup):
    waiting_for_text = State()

# ===================== ОБРАБОТЧИК КОМАНДЫ /start =====================
@dp.message(Command("start"))
async def cmd_start(message: types.Message):
    await message.answer(
        f"👋 Привет, {message.from_user.first_name}!\n\n"
        "Я бот проекта **VIBEs для молодежи**.\n"
        "Мы сопровождаем молодых людей в возрасте от 15 до 31 года\n"
        "в поиске своего пути, получении профессиональных навыков\n"
        "и оказываем содействие в трудоустройстве.\n\n"
        "✅ Участие в проекте **бесплатное**.\n"
        "✅ Помогаем с профориентацией, обучением, трудоустройством.\n\n"
        "Выберите интересующий Вас раздел в меню ниже 👇",
        reply_markup=main_keyboard,
        parse_mode="Markdown"
    )

# ===================== БЛОК "ДЛЯ КОГО ПРОЕКТ?" =====================
@dp.message(F.text == "👥 Для кого проект?")
async def who_project(message: types.Message):
    await message.answer(
        "👥 **Кому помогает проект VIBEs?**\n\n"
        "Проект создан для молодых людей в возрасте **от 15 до 31 года**, "
        "которые находятся в трудной жизненной ситуации:\n\n"
        "• Дети-сироты и дети, оставшиеся без попечения родителей\n"
        "• Одинокие родители\n"
        "• Молодые люди с инвалидностью или особенностями психофизического развития\n"
        "• Те, кто столкнулся с безработицей, болезнью или низкой материальной обеспеченностью\n"
        "• Молодые люди в социально опасном положении\n\n"
        "Если Вы или Ваши знакомые попадаете в эти категории — **этот проект для Вас!**",
        parse_mode="Markdown"
    )

# ===================== БЛОК "ЧЕМ ПОЛЕЗЕН ПРОЕКТ?" =====================
@dp.message(F.text == "✨ Чем полезен проект?")
async def benefits_project(message: types.Message):
    await message.answer(
        "✨ **Что даёт участие в проекте VIBEs?**\n\n"
        "🔹 **Тренинговая программа** – развиваем жизненные навыки и навыки трудоустройства\n"
        "🔹 **Консультации** – помогаем выбрать профессию и построить карьеру\n"
        "🔹 **Психологическая поддержка** – сопровождаем на всём пути проекта\n"
        "🔹 **Обучение** – направляем на профессиональные курсы\n"
        "🔹 **Практики и стажировки** – даём возможность получить опыт работы\n"
        "🔹 **Инструменты** – обеспечиваем необходимым для профессиональной деятельности\n\n"
        "Каждый участник получает **индивидуальную поддержку** от кейс-менеджера, "
        "который будет рядом на всех этапах проекта.",
        parse_mode="Markdown"
    )

# ===================== БЛОК "УЧАСТНИКАМ" =====================
@dp.message(F.text == "ℹ️ Участникам")
async def info_beneficiary(message: types.Message):
    await message.answer(
        "ℹ️ **Информация для участния в проекте VIBEs**\n\n"
        "Проект VIBEs — это бесплатная помощь молодым людям, "
        "которые оказались в трудной жизненной ситуации.\n\n"
        "**Для участия в проекте Вам необходимо:**\n"
        "✓ подтвердить документами свою трудную жизенную ситуацию\n"
        "✓ написать заявление, подписать соглашение и информационнное согласие\n"
        "✓ с нашей помощью составить индивидуальный план участия в проекте\n"
        "✓ активно участвовать в мероприятиях проекта для достижения своих личных целей\n\n"
        "**Для этого:**\n"
        "1. Позвоните нам по телефону: **+375 29 508-02-60** (Ольга)\n"
        "2. Или нажмите кнопку **«📝 Подать заявку»** ниже — мы свяжемся с Вами.",
        parse_mode="Markdown"
    )

# ===================== БЛОК "ЮРИДИЧЕСКИМ ЛИЦАМ" =====================
@dp.message(F.text == "🤝 Юридическим лицам")
async def info_partners(message: types.Message):
    await message.answer(
        "🤝 **Приглашение к сотрудничеству юридических лиц**\n\n"
        "SOS-Детские деревни Могилёв приглашает специалистов и организации к сотрудничеству "
        "в рамках проекта «VIBEs для молодежи». Мы открыты к партнёрству с:\n\n"
        "• Частными компаниями и корпорациями\n"
        "• Государственными учреждениями\n"
        "• Неправительственными организациями\n"
        "• Образовательными центрами\n"
        "• Епархиями\n\n"
        "**Формы сотрудничества:**\n"
        "• Совместная реализация проектных мероприятий\n"
        "• Обучение специалистов программе ЖНТ\n"
        "• Экспертная поддержка и наставничество\n\n"
        "**По вопросам партнёрства обращайтесь:**\n"
        "📞 **Телефон:** +375 29 508-02-60 (Ольга Анатольевна)\n"
        "📧 **E-mail:** kupcoiric@mail.ru",
        parse_mode="Markdown"
    )

# ===================== БЛОК "ВИДЕО О ПРОЕКТЕ" =====================
@dp.message(F.text == "▶️ Видео о проекте")
async def video_project(message: types.Message):
    await message.answer(
        "▶️ **Смотрите видео о проекте VIBEs:**\n\n"
        "• **Обзор проекта:** [Посмотреть на YouTube](https://www.youtube.com/watch?v=JAXCQlezEI4)\n\n"
        "🎥 **Истории успеха:**\n"
        "• [История 1](https://youtu.be/oMhERVLU2D8?si=AMYAFwALMLi6Jfst)\n"
        "• [История 2](https://youtu.be/aMHgycnzjBM?si=3cAr7lmL04VVNuuL)\n"
        "• [История 3](https://youtu.be/IhEoGYjL1uw?si=y0K_2LFEToPseZXP)\n\n"
        "Подписывайтесь на наш YouTube-канал, чтобы не пропустить новые видео!",
        parse_mode="Markdown",
        disable_web_page_preview=True
    )

# ===================== БЛОК "МЕРОПРИЯТИЯ" =====================
@dp.message(F.text == "📅 Мероприятия")
async def events_list(message: types.Message):
    # Здесь вы можете редактировать список мероприятий.
    # Добавляйте новые события в этот список, следуя формату.
    events_text = (
        "📅 **Жизнь проекта (здесь будут фото)**\n\n"
              "❗ Следите за обновлениями — мероприятия добавляются регулярно.\n"

    )
    await message.answer(events_text, parse_mode="Markdown")

# ===================== БЛОК "ЗАДАТЬ ВОПРОС" =====================
@dp.message(F.text == "❓ Задать вопрос")
async def ask_question(message: types.Message, state: FSMContext):
    await state.set_state(QuestionState.waiting_for_text)
    await message.answer(
        "❓ **Задать вопрос координатору проекта**\n\n"
        "Напишите Ваш вопрос. Постарайтесь изложить его чётко.\n"
        "Координатор проекта ответит Вам на указанную в заявке почту или свяжется по телефону.\n"
        "Укажите после вопроса своё имя, адрес электронной почты или номер телефона для связи.\n\n"
        "✏️ Введите текст вопроса:",
        parse_mode="Markdown",
        reply_markup=ReplyKeyboardRemove()  # убираем клавиатуру, чтобы не мешала
    )

# ===================== ОБРАБОТЧИК ПОЛУЧЕНИЯ ТЕКСТА ВОПРОСА =====================
@dp.message(StateFilter(QuestionState.waiting_for_text))
async def process_question(message: types.Message, state: FSMContext):
    question_text = message.text
    user_name = message.from_user.full_name
    user_id = message.from_user.id
    # Формируем письмо с вопросом
    subject = "Вопрос от участника проекта VIBEs"
    body = (
        f"Поступил новый вопрос от пользователя Telegram:\n\n"
        f"👤 Имя: {user_name}\n"
        f"🆔 ID: {user_id}\n"
        f"📝 Вопрос:\n{question_text}\n\n"
        "Ответьте на вопрос, используя контактные данные, если они известны, "
        "или свяжитесь с участником через Telegram."
    )

    try:
        # Отправляем письмо
        msg = MIMEMultipart()
        msg["From"] = SMTP_LOGIN
        msg["To"] = "kupcoiric@mail.ru"
        msg["Subject"] = subject
        msg.attach(MIMEText(body, "plain", "utf-8"))

        server = smtplib.SMTP(SMTP_SERVER, SMTP_PORT)
        server.starttls()
        server.login(SMTP_LOGIN, SMTP_PASSWORD)
        server.send_message(msg)
        server.quit()

        await message.answer(
            "✅ **Ваш вопрос отправлен!**\n\n"
            "Куратор свяжется с Вами в ближайшее время.\n"
            "Спасибо за обращение! 🌟",
            parse_mode="Markdown",
            reply_markup=main_keyboard
        )
    except Exception as e:
        logging.error(f"Ошибка отправки вопроса: {e}")
        await message.answer(
            "❌ **Произошла ошибка при отправке вопроса.**\n\n"
            "Пожалуйста, свяжитесь с нами напрямую:\n"
            "📞 +375 29 508-02-60 (Ольга Анатольевна)",
            parse_mode="Markdown",
            reply_markup=main_keyboard
        )

    await state.clear()


# ===================== НАЧАЛО ЗАЯВКИ =====================
@dp.message(F.text == "📝 Подать заявку")
async def start_application(message: types.Message, state: FSMContext):
    await state.set_state(ApplicationForm.name)
    await message.answer(
        "📝 **Заполнение заявки на участие в проекте VIBEs**\n\n"
        "Я задам Вам 5 вопросов, чтобы передать заявку куратору.\n"
        "Отвечайте коротко и по делу.\n\n"
        "**Вопрос 1 из 5:** Как Вас зовут?",
        parse_mode="Markdown",
        reply_markup=ReplyKeyboardRemove()
    )

# ===================== ВОПРОС 1: ИМЯ =====================
@dp.message(StateFilter(ApplicationForm.name))
async def process_name(message: types.Message, state: FSMContext):
    await state.update_data(name=message.text)
    await state.set_state(ApplicationForm.age)
    await message.answer(
        "**Вопрос 2 из 5:** Сколько Вам лет?",
        parse_mode="Markdown"
    )

# ===================== ВОПРОС 2: ВОЗРАСТ =====================
@dp.message(StateFilter(ApplicationForm.age))
async def process_age(message: types.Message, state: FSMContext):
    await state.update_data(age=message.text)
    await state.set_state(ApplicationForm.phone)
    await message.answer(
        "**Вопрос 3 из 5:** Укажите Ваш номер телефона для связи",
        parse_mode="Markdown"
    )

# ===================== ВОПРОС 3: ТЕЛЕФОН =====================
@dp.message(StateFilter(ApplicationForm.phone))
async def process_phone(message: types.Message, state: FSMContext):
    await state.update_data(phone=message.text)
    await state.set_state(ApplicationForm.situation)
    await message.answer(
        "**Вопрос 4 из 5:** Опишите Вашу трудную жизненную ситуацию:",
        parse_mode="Markdown"
    )

# ===================== ВОПРОС 4: СИТУАЦИЯ =====================
@dp.message(StateFilter(ApplicationForm.situation))
async def process_situation(message: types.Message, state: FSMContext):
    await state.update_data(situation=message.text)
    await state.set_state(ApplicationForm.documents)
    await message.answer(
        "**Вопрос 5 из 5:** Можете ли Вы подтвердить свою ситуацию документально? (если да, то какие документы у Вас есть?)",
        parse_mode="Markdown"
    )

# ===================== ВОПРОС 5: ДОКУМЕНТЫ =====================
@dp.message(StateFilter(ApplicationForm.documents))
async def process_documents(message: types.Message, state: FSMContext):
    await state.update_data(documents=message.text)
    data = await state.get_data()

    await message.answer(
        "📋 **Проверьте введённые данные:**\n\n"
        f"👤 Имя: {data['name']}\n"
        f"📅 Возраст: {data['age']}\n"
        f"📞 Телефон: {data['phone']}\n"
        f"📝 Ситуация: {data['situation']}\n"
        f"📄 Документы: {data['documents']}\n\n"
        "Всё верно? Нажмите **«✅ Отправить»** или **«❌ Отменить»**.",
        parse_mode="Markdown",
        reply_markup=ReplyKeyboardMarkup(
            keyboard=[
                [KeyboardButton(text="✅ Отправить")],
                [KeyboardButton(text="❌ Отменить")]
            ],
            resize_keyboard=True
        )
    )
    await state.set_state(ApplicationForm.confirm)

   
# ===================== ОТПРАВКА ЗАЯВКИ (без изменений) =====================
@dp.message(StateFilter(ApplicationForm.confirm), F.text == "✅ Отправить")
async def send_application(message: types.Message, state: FSMContext):
    data = await state.get_data()

    subject = "Новая заявка на участие в проекте VIBEs"
    body = (
        f"Поступила новая заявка от участника:\n\n"
        f"👤 Имя: {data['name']}\n"
        f"📅 Возраст: {data['age']}\n"
        f"📞 Телефон: {data['phone']}\n"
        f"📝 Ситуация: {data['situation']}\n\n"
        f"📝 Документы: {data['ocuments']}\n\n"
        f"Связаться с участником можно по указанному телефону."
    )

    try:
        msg = MIMEMultipart()
        msg["From"] = SMTP_LOGIN
        msg["To"] = "kupcoiric@mail.ru"
        msg["Subject"] = subject
        msg.attach(MIMEText(body, "plain", "utf-8"))

        server = smtplib.SMTP(SMTP_SERVER, SMTP_PORT)
        server.starttls()
        server.login(SMTP_LOGIN, SMTP_PASSWORD)
        server.send_message(msg)
        server.quit()

        await message.answer(
            "✅ **Ваша заявка успешно отправлена!**\n\n"
            "Мы свяжемся с Вами.\n"
            "Спасибо, что выбрали проект VIBEs! 🌟",
            parse_mode="Markdown",
            reply_markup=main_keyboard
        )
    except Exception as e:
        logging.error(f"Ошибка отправки заявки: {e}")
        await message.answer(
            "❌ **Произошла ошибка при отправке заявки.**\n\n"
            "Пожалуйста, свяжитесь с нами напрямую:\n"
            "📞 +375 29 508-02-60 (Ольга Анатольевна)",
            parse_mode="Markdown",
            reply_markup=main_keyboard
        )

    await state.clear()

# ===================== ОТМЕНА ЗАЯВКИ =====================
@dp.message(StateFilter(ApplicationForm.confirm), F.text == "❌ Отменить")
async def cancel_application(message: types.Message, state: FSMContext):
    await state.clear()
    await message.answer(
        "❌ Заявка отменена.\n\n"
        "Если передумаете — всегда можете подать заявку снова через кнопку "
        "«📝 Подать заявку» в главном меню.",
        reply_markup=main_keyboard
    )

# ===================== ОБРАБОТЧИК ВСЕХ ОСТАЛЬНЫХ СООБЩЕНИЙ =====================
@dp.message()
async def handle_other_messages(message: types.Message):
    await message.answer(
        "Я Вас не совсем понял 😅\n\n"
        "Пожалуйста, воспользуйтесь кнопками в меню ниже 👇",
        reply_markup=main_keyboard
    )

# ===================== ЗАПУСК БОТА =====================
async def main():
    print("🤖 Бот VIBEs запущен и готов к работе!")
    await dp.start_polling(bot)

if __name__ == "__main__":
    asyncio.run(main())
