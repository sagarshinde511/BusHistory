import streamlit as st
import mysql.connector
import pandas as pd

# Database connection details
host = "82.180.143.66"
user = "u263681140_students"
passwd = "testStudents@123"
db_name = "u263681140_students"

def fetch_data():
    try:
        # Connect to the MySQL database
        conn = mysql.connector.connect(
            host=host,
            user=user,
            password=passwd,
            database=db_name
        )

        cursor = conn.cursor()

        # SQL query to join BusHistory and BusPassengers tables
        query = """
        SELECT 
            BusHistory.DateTime, 
            BusHistory.RFidNo, 
            BusHistory.inLocation, 
            BusHistory.outLocation, 
            BusHistory.distance, 
            BusHistory.amount, 
            BusPassengers.Name, 
            BusPassengers.Gender
        FROM 
            BusHistory
        INNER JOIN 
            BusPassengers
        ON 
            BusHistory.RFidNo = BusPassengers.RFid;
        """

        # Execute the query
        cursor.execute(query)

        # Fetch all the rows
        rows = cursor.fetchall()

        # Get column names
        columns = [desc[0] for desc in cursor.description]

        # Convert to a Pandas DataFrame
        data = pd.DataFrame(rows, columns=columns)

        return data

    except mysql.connector.Error as e:
        st.error(f"Error connecting to the database: {e}")
        return None

    finally:
        if 'conn' in locals() and conn.is_connected():
            cursor.close()
            conn.close()

# Streamlit app
st.title("Bus History and Passengers Data Viewer")

# Fetch data
data = fetch_data()

if data is not None:
    st.write("### Combined Data from BusHistory and BusPassengers")
    st.dataframe(data)
else:
    st.write("No data available.")
