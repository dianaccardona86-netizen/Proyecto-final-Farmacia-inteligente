from flask import Flask, render_template, request, jsonify

app = Flask(__name__)

current_data = {

    "temp_amb": 0,
    "humedad": 0,
    "temp_gab": 0,
    "aire": 0,
    "luz": 0,
    "puerta": "Cerrada",
    "ventilacion": "Apagada",
    "rgb": "Verde",
    "alarma": "Normal",
    "sistema": "Desconectado"

}

@app.route('/')
def index():

    return render_template('index.html')

# ==========================================
# RECIBE LOS DATOS DE LA ESP32
# ==========================================

@app.route('/update', methods=['POST'])
def update():

    global current_data

    current_data = request.json

    return "OK", 200

# ==========================================
# ENTREGA LOS DATOS AL HTML
# ==========================================

@app.route('/api/data')
def get_data():

    return jsonify(current_data)

if __name__ == '__main__':

    print("Servidor iniciado")

    app.run(
        host='0.0.0.0',
        port=5000
    )
