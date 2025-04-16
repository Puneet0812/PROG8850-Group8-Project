import threading
import mysql.connector

def run_query(query):
    try:
        conn = mysql.connector.connect(
            host="localhost",
            user="root",
            password="Rambo@123",
            database="project_db"
        )
        cursor = conn.cursor()
        cursor.execute(query)
        if query.lower().startswith("select"):
            print(cursor.fetchall())
        conn.commit()
        cursor.close()
        conn.close()
    except Exception as e:
        print("❌ Error:", e)

queries = [
    "INSERT INTO ClimateData (location, record_date, temperature, precipitation, humidity) VALUES ('Pune', '2024-05-02', 32.0, 3.0, 55.0)",
    "UPDATE ClimateData SET humidity = 50.0 WHERE location = 'Delhi'",
    "SELECT * FROM ClimateData WHERE temperature > 20"
]

threads = []
for q in queries:
    t = threading.Thread(target=run_query, args=(q,))
    t.start()
    threads.append(t)

for t in threads:
    t.join()
