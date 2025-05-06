# AI-Cold-Calling-Agent

Outbound Call Agent

This project is a Node.js application built with Fastify, Twilio, and ElevenLabs to create an automated outbound call system. It allows you to initiate phone calls to customers, using a conversational AI agent powered by ElevenLabs to interact with them in real-time. The system is designed for scenarios like sales calls, customer support, or automated outreach.

Features





Outbound Call Initiation: Trigger calls to customers via a REST API endpoint.



Real-Time Voice Interaction: Uses ElevenLabs' Conversational AI to handle voice conversations dynamically.



Twilio Integration: Manages telephony operations, including call initiation and media streaming.



WebSocket Support: Handles real-time audio streams between Twilio and ElevenLabs.



Customizable Prompts: Configure the AI agent's behavior and initial message for each call.



Environment Variable Configuration: Securely manage API keys and credentials using a .env file.

Prerequisites

Before running the application, ensure you have the following:





Node.js (v16 or higher)



npm (Node Package Manager)



Twilio Account with a phone number configured



ElevenLabs Account with access to the Conversational AI API



Ngrok (optional, for exposing your local server to the internet during development)

Installation





Clone the Repository:

git clone <repository-url>
cd outbound-call-agent



Install Dependencies:

npm install



Set Up Environment Variables: Create a .env file in the root directory and add the following:

ELEVENLABS_API_KEY=your_elevenlabs_api_key
ELEVENLABS_AGENT_ID=your_elevenlabs_agent_id
TWILIO_ACCOUNT_SID=your_twilio_account_sid
TWILIO_AUTH_TOKEN=your_twilio_auth_token
TWILIO_PHONE_NUMBER=your_twilio_phone_number
PORT=8000

Replace the placeholders with your actual credentials from Twilio and ElevenLabs.



Expose Your Local Server (Optional): If running locally, use Ngrok to expose your server to the internet:

ngrok http 8000

Note the Ngrok URL (e.g., https://<ngrok-id>.ngrok-free.app) for use in API calls.

Usage





Start the Server:

npm start

The server will run on http://localhost:8000 (or the port specified in .env).



Health Check: Verify the server is running by accessing:

GET http://localhost:8000/

Expected response:

{ "message": "Server is running" }



Initiate an Outbound Call: Use the /outbound-call endpoint to start a call. Example using curl:

curl -X POST https://<your-ngrok-url-or-domain>/outbound-call \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "You are Eric, an outbound car sales agent. Be friendly and professional and answer all questions.",
    "first_message": "Hello, my name is Eric! I heard you were looking for a new car. What model and color are you interested in?",
    "number": "+1234567890"
  }'





prompt: Defines the AI agent's behavior.



first_message: The initial message spoken by the AI.



number: The customer's phone number in international format (e.g., +1234567890).

Expected response:

{
  "success": true,
  "message": "Call initiated",
  "callSid": "<twilio-call-sid>"
}



WebSocket Stream: The server uses WebSockets to handle real-time audio streams between Twilio and ElevenLabs. The /outbound-media-stream endpoint is automatically used by Twilio during calls.

Endpoints





GET /: Health check endpoint to verify server status.



POST /outbound-call: Initiates an outbound call with the provided phone number, prompt, and first message.



ALL /outbound-call-twiml: Generates TwiML for Twilio to connect the call to a WebSocket stream.



GET /outbound-media-stream: WebSocket endpoint for handling real-time audio streams.

Project Structure

├── .env                 # Environment variables
├── index.js            # Main application file
├── package.json        # Project dependencies and scripts
├── README.md           # Project documentation
└── node_modules/       # Installed dependencies

Dependencies





Fastify: A fast and low-overhead web framework for Node.js.



@fastify/websocket: Plugin for WebSocket support in Fastify.



@fastify/formbody: Plugin for parsing form data in Fastify.



Twilio: SDK for telephony operations.



WebSocket: Library for WebSocket communication with ElevenLabs.



dotenv: Loads environment variables from a .env file.

Environment Variables







Variable



Description



Required





ELEVENLABS_API_KEY



API key for ElevenLabs Conversational AI



Yes





ELEVENLABS_AGENT_ID



Agent ID for ElevenLabs Conversational AI



Yes





TWILIO_ACCOUNT_SID



Twilio Account SID



Yes





TWILIO_AUTH_TOKEN



Twilio Auth Token



Yes





TWILIO_PHONE_NUMBER



Twilio phone number for outbound calls



Yes





PORT



Port for the Fastify server (default: 8000)



No

Troubleshooting





Ngrok URL Issues: Ensure your Ngrok tunnel is active and the URL matches in your API calls.



Twilio Errors: Verify your Twilio credentials and phone number are correct. Check Twilio's console for detailed error logs.



ElevenLabs Errors: Ensure your API key and agent ID are valid. Check the ElevenLabs API documentation for rate limits or restrictions.



WebSocket Disconnections: Confirm your server is accessible and not behind a firewall blocking WebSocket connections.
