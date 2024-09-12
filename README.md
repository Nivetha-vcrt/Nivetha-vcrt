pip install Flask
# app.py
from flask import Flask, request, jsonify, render_template

app = Flask(__name__)

# Sample data storage
appointments = []
medications = []

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/schedule', methods=['POST'])
def schedule_appointment():
    data = request.json
    appointments.append(data)
    return jsonify({"message": "Appointment scheduled successfully!"})

@app.route('/appointments', methods=['GET'])
def get_appointments():
    return jsonify(appointments)

@app.route('/medication', methods=['POST'])
def add_medication():
    data = request.json
    medications.append(data)
    return jsonify({"message": "Medication reminder set!"})

@app.route('/medications', methods=['GET'])
def get_medications():
    return jsonify(medications)
    <!-- templates/index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Patient Engagement Platform</title>
</head>
<body>
    <h1>Patient Engagement Platform</h1>
    <form id="appointment-form">
        <h2>Schedule Appointment</h2>
        <input type="text" id="appointment-date" placeholder="Date" required>
        <input type="text" id="appointment-time" placeholder="Time" required>
        <button type="button" onclick="scheduleAppointment()">Schedule</button>
    </form>

    <form id="medication-form">
        <h2>Set Medication Reminder</h2>
        <input type="text" id="medication-name" placeholder="Medication Name" required>
        <input type="text" id="medication-time" placeholder="Time" required>
        <button type="button" onclick="addMedication()">Add Reminder</button>
    </form>

    <script>
        function scheduleAppointment() {
            const date = document.getElementById('appointment-date').value;
            const time = document.getElementById('appointment-time').value;
            fetch('/schedule', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ date, time })
            })
            .then(response => response.json())
            .then(data => alert(data.message));
        }

        function addMedication() {
            const name = document.getElementById('medication-name').value;
            const time = document.getElementById('medication-time').value;
            fetch('/medication', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ name, time })
            })
            .then(response => response.json())
            .then(data => alert(data.message));
        }
    </script>
</body>
</html>
python app.py


if __name__ == '__main__':
    app.run(debug=True)

