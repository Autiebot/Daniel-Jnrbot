# Daniel-Jnrbot
Trading bot 
import asyncio
import os
from metaapi_cloud_sdk import MetaApi

token = os.getenv("TOKEN")
account_id = os.getenv("ACCOUNT_ID")

api = MetaApi(token)

async def run_bot():
    account = await api.metatrader_account_api.get_account(account_id)
    connection = account.get_rpc_connection()
    await connection.connect()
    await connection.wait_synchronized()

    print("Bot started...")

    while True:
        try:
            positions = await connection.get_positions()
            print(f"Open positions: {positions}")

            await asyncio.sleep(60)

        except Exception as e:
            print(f"Error: {e}")
            await asyncio.sleep(10)

asyncio.run(run_bot())
