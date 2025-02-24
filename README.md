# tareas-trabajo

import streamlit as st
import openai

# Configurar la clave de API de OpenAI
openai.api_key = "TU_API_KEY"

def chatbot_response(user_input):
    try:
        response = openai.ChatCompletion.create(
            model="gpt-3.5-turbo",
            messages=[{"role": "system", "content": "Eres un asistente útil."},
                      {"role": "user", "content": user_input}]
        )
        return response["choices"][0]["message"]["content"]
    except Exception as e:
        return f"Error: {str(e)}"

# Interfaz con Streamlit
st.title("Chatbot Online con GPT-3.5")
st.write("Este es un chatbot interactivo basado en OpenAI GPT-3.5.")

if "chat_history" not in st.session_state:
    st.session_state.chat_history = []

user_input = st.text_input("Escribe tu mensaje:", "")

if st.button("Enviar") and user_input:
    response = chatbot_response(user_input)
    st.session_state.chat_history.append(("Tú", user_input))
    st.session_state.chat_history.append(("Bot", response))

st.write("## Conversación")
for sender, message in st.session_state.chat_history:
    st.write(f"**{sender}:** {message}")
