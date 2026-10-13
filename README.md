# StrathClean

Second place at StrathHack 2026, built in 24 hours by [Elodie](https://github.com/elodiegrb), [Llywelyn](https://github.com/Llywelyn-Devoy), [Scott](https://github.com/ScottCowe), and [Me](https://github.com/FindlayT).

Strathclyde’s cleaning team works from schedules across buildings. Supervisors audit rooms after they are cleaned. The existing app asks for a lot of manual data entry, and it does not know which rooms exist, when they were cleaned, or who cleaned them. StrathClean is a Flutter app that addresses these issues. You can log a clean in a few taps, find the room from your current GPS location, and only enter data when there is an issue.

## What you can do

You can sign in with an account that is either a cleaner or supervisor.

**Cleaners**

- Log that a room was cleaned. No data entry is required unless there is an issue.
- Log an external issue (PAT testing, door locks, damaged furniture, building kit, etc.) with a description, priority (low, medium, high), and an optional photo.
- Look up rooms and open issues.

**Supervisors**

- Supervisors click begin audit which then displays up to five rooms at random whose latest clean has not been checked. Each row shows who cleaned it, when they cleaned it, any earlier issue, and issues still open.
- Audit that room. Leaving it as a pass records the audit. Marking a failure asks for a tag (cleaning or external), a priority, a description, and an optional photo.
- Log issues, then resolve or edit them.
- Add and edit rooms: name, building, floor, notes, and a map outline.

**Finding a room**

- Automatically Detect Nearest Room: Uses the devices GPS to find the closest KML outline of a room and auto fill all the info for that room. A supervisor draws the outline on the map when they add or edit a room.
- Automatically Scan Room Number: Scan room code reads a door label with the devices camera (a short letter prefix and three or four digits, such as `AB123`).
- Manually enter room: Filter by building and floor, or fuzzy search the room name.


## Stack

App: Flutter (web and Android).

Data: PostgreSQL, schema defined in code by [Drift](https://drift.simonbinder.eu/) (`lib/database/app_database.dart`)

Tables: `users`, `rooms`, `cleans`, `issues`. Drift creates and migrates them the first time the app connects.

## How  to run the app

You need Flutter, the Dart SDK, and a Postgres database.

Create `.env` in the project root:

```
DB_HOST=your_postgres_server_address_here
DB_PORT=5432
DB_NAME=your_db_name_here
DB_USER=your_user_here
DB_PASSWORD=your_password_here
```

To Start the web app:

```bash
# macOS / Linux
./run_web.sh

# Windows
run_web.bat
``` 

To Start the android app:

```bash
dart run bin/db_proxy.dart
```

then in another session:

```bash
flutter run
```

you may need to do this to make sure your phone can access the database proxy:

```bash
adb reverse tcp:8081 tcp:8081
```

There is no starting data. After the proxy has created the tables, you will have to go in and create users with a manual sql query:

```sql
insert into users (role, username, password_hash) values
  ('cleaner', 'alex', 'password'),
  ('supervisor', 'sophie', 'password');
```

Sign in as the supervisor and add rooms first, then the cleaner account will be able to log cleans.
