## 中文简洁说明Chinese
原项目获取验证码并自动预定有点问题. 由于我没仔细研究,所以增加了以下功能
1. 运行 python main.py 后, 如果有合适的时间段, 会让你电脑发出声音. 目的相当于是双保险. 比如你在刷剧,听到声音后可以手动打开, 并手动预定时间段.
2. 期待你的提建议. 我会的我就会帮忙做. 我的邮箱 loading800@gmail.com
3. 好心的朋友能提供机会让我参加全栈项目吗? 纯小白,但是我想入行了..坐标加拿大.
4. 有糊口的工作也可以叫我. 身体健康不吸烟不喝酒,可做重活. 灵活. 没有不良癖好习惯.全国可飞.
5. 刚来加拿大BC温哥华,目前在ICBC跳考Class 5.时间等太久就用这个预约到了.(哭死,第一次路考前几天路考翻车,继续用本软件预约到3周后的.)

## English Brief
1. Run `python main.py`, and if there’s a good time slot, your computer’s gonna beep. It’s like a backup plan—if you’re binge-watching, you’ll hear it, pop open the program, and book the slot yourself.
2. Hit me with your suggestions! I’ll help with whatever I can. My email’s loading800@gmail.com.
3. Any nice folks out there got a full-stack project I could jump into? Total newbie, but I’m dying to get into it. I’m in Canada.
4. Also, if there’s any job to pay the bills, I’m your guy. Healthy, don’t smoke or drink, can handle tough work, super flexible, no bad habits. I’ll fly anywhere in the country.
5. Just landed in Vancouver, BC, and I’m prepping for my ICBC Class 5 road test. Wait times were crazy, so I used this program to snag a slot. (Ugh, crashed and burned my first road test a few days ago, but I booked another one for three weeks out with this tool.)

# ICBC Road Test Catcher

An automated Python script that monitors and books available road test appointments at ICBC (Insurance Corporation of British Columbia) locations in Canada.

This repository has been converted to a pure Python project.


## Features

- Automated Monitoring: Continuously checks for available appointments every 90 seconds
- Smart Filtering: Only considers appointments within your specified date range
- Full Automation: Complete booking process without manual intervention
- Email Integration: Automatically retrieves and processes OTP codes from Gmail
- Token Management: Handles authentication token refresh automatically
- Multi-location Support: Can monitor multiple ICBC locations simultaneously
- Error Handling: Robust error handling with retry mechanisms
- Timezone Aware: Properly configured for Pacific Time (America/Vancouver)

## How It Works

1. Authentication: Logs into ICBC system using your credentials
2. Monitoring: Continuously scans for available appointments
3. Booking Process: When a suitable slot is found:
   - Locks the appointment temporarily and Alert sound from your PC!
   - Requests OTP code via email
   - Automatically retrieves OTP from Gmail
   - Verifies the OTP code
   - Completes the booking

## Prerequisites

- Python 3.7+
- Gmail account with App Password enabled
- Valid ICBC learner's license and credentials

## Installation (Local Python)

1. Clone the repository:

```bash
git clone https://github.com/elonhsiao/icbc-slot-finder.git
cd icbc-slot-finder
```

2. (Recommended) Create and activate a virtual environment:

```bash
python -m venv .venv
.\.venv\Scripts\Activate.ps1   # PowerShell on Windows
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

4. Create a `.env` file in the project root (or set environment variables) with the required values described below.

## Configuration

Set the following environment variables (either in a `.env` file or in your shell):

- USER_LAST_NAME: Your last name
- USER_LICENSE_NUMBER: Your license number
- USER_KEYWORD: Your ICBC keyword
- USER_GMAIL: Your Gmail address
- USER_GMAIL_APP_PASSWORD: 16-character Gmail App Password (IMAP access)
- DESIRED_DATE_START (optional): Start date in YYYY-MM-DD format (default in code)
- DESIRED_DATE_END (optional): End date in YYYY-MM-DD format (default in code)
- LOCATION_IDS (optional): Comma-separated list of ICBC location IDs (default: 214)

Example `.env` file:

```
USER_LAST_NAME=YourLastName
USER_LICENSE_NUMBER=1234567
USER_KEYWORD=your_keyword_here
USER_GMAIL=your.email@gmail.com
USER_GMAIL_APP_PASSWORD=your_16_character_app_password
DESIRED_DATE_START=2025-06-24
DESIRED_DATE_END=2025-06-30
LOCATION_IDS=214
```

**Note**: You must enable 2-factor authentication on Gmail and generate an App Password. Regular Gmail passwords won't work.

## Usage

Run the script locally:

```powershell
python main.py
```

What the script does:
- Starts monitoring immediately
- Displays status updates in the console
- Automatically books when a suitable appointment is found
- Stops execution after successful booking
 - Plays an audible alert (continuous beep) when an appointment is found so you notice immediately

Alert sound
-----------------
When the script finds a suitable appointment it starts a continuous audible alert (a repeated beep). The script then waits for the user to press Enter; when you press Enter the alert stops and the script exits.

Notes about the alert:
- The alert is implemented using the Windows `winsound.Beep()` API (so it will sound on Windows). If you run the script on a different OS the alert may not play.
- To stop the alert: press Enter in the console where the script is running. The script will signal the sound thread to stop and then exit.
- To disable the audible alert permanently, edit `main.py` and comment out the alert thread start/stop block in `auto_book_earliest_appointment()` (the lines that create `alert_thread`, call `alert_thread.start()`, wait for input, call `stop_alert.set()` and `alert_thread.join()`).


## Sample Output

```
Token refreshed. drvrID: 12345678
Script started. Beginning monitoring for available dates...
Found 3 available dates for location 214
Date 2025-06-25 successfully locked
OTP code sent to email
OTP code successfully verified
Booking completed successfully!
```

## Location IDs

Common ICBC locations and their IDs:
- Duncan: 214
- Abbotsford: 1
If you don't know how to find Location IDs, feel free to contact me. :)
如果你不知道怎么找到Location ID, 可以联系我. :)

To find other location IDs, inspect network traffic in your browser when selecting different locations on the ICBC website.

## Important Notes

- Rate Limiting: The script respects ICBC's servers with reasonable intervals
- Legal Compliance: This tool automates the same process you would do manually
- Single Use: Script stops after successfully booking one appointment
- Timezone: Configured for Pacific Time (America/Vancouver)

## Troubleshooting

### Common Issues

1. Authentication Failed
   - Verify your ICBC credentials are correct
   - Check that your license is valid and active

2. Gmail Connection Issues
   - Ensure 2FA is enabled on Gmail
   - Verify App Password is correctly generated and entered
   - Check IMAP is enabled in Gmail settings

3. No Appointments Found
   - Adjust your date range
   - Add more location IDs
   - Check if appointments are available manually on ICBC website

4. Token Refresh Errors
   - Usually resolves automatically on next refresh cycle
   - Restart script if persistent

## Disclaimer

This tool is for educational purposes and personal use only. Users are responsible for:
- Complying with ICBC terms of service
- Using the tool responsibly and ethically
- Not overloading ICBC servers

## Original Author

**steven.marelly:** 
**My Email:** loading800@gmail.com
