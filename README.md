# telegram_scraper
from telethon.sync import TelegramClient


api_id = '21078682'  
api_hash = '9a66111725984f3c6c4231d0a0348f94'  
phone_number = '+2349168062578'


client = TelegramClient(phone_number, api_id, api_hash)


async def scrape_members(channel_username):
    await client.start()
    print("Client connected successfully")

    
    try:
        channel = await client.get_entity(channel_username)
    except Exception as e:
        print(f"Error fetching channel: {e}")
        return

    print(f"Fetching members for {channel.title}...")


    participants = await client.get_participants(channel)

    
    with open("channel_members.csv", "w", encoding="utf-8") as file:
        file.write("ID, Username, First Name, Last Name\n")  # CSV Header
        for participant in participants:
            user_id = participant.id
            username = participant.username if participant.username else "N/A"
            first_name = participant.first_name if participant.first_name else "N/A"
            last_name = participant.last_name if participant.last_name else "N/A"
            print(f"User ID: {user_id} | Username: {username} | First Name: {first_name} | Last Name: {last_name}")

            
            file.write(f"{user_id}, {username}, {first_name}, {last_name}\n")

    print("Member list saved to 'channel_members.csv'")
    await client.disconnect()


with client:
    client.loop.run_until_complete(scrape_members('https://t.me/FreeBook_Hub'))
