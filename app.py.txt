from flask import Flask, jsonify, render_template
import socket

app = Flask(__name__)

LMS_IP = "localhost"
LMS_PORT = 9090

def lms_command(command):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect((LMS_IP, LMS_PORT))
    sock.send((command + "\n").encode('utf-8'))
    response = sock.recv(4096).decode('utf-8')
    sock.close()
    return response

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/pause')
def pause():
    lms_command("pause")
    return jsonify({"status": "ok"})

@app.route('/play')
def play():
    lms_command("play")
    return jsonify({"status": "ok"})

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5001)
