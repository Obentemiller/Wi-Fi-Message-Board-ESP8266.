#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>
#include <WiFiClient.h>

ESP8266WebServer server(80);

String chatLog = "";
String userName = "Usuário"; // Default username

void setup() {
  // Initialize ESP8266 in access point mode without a password
  String ssid = "acesse 192.168.4.1";
  WiFi.softAP(ssid.c_str());

  // Set up routes for the chat page and message handling
  server.on("/", HTTP_GET, handleChat);
  server.on("/chat", HTTP_POST, handleChatMessage);

  // Initialize the web server
  server.begin();

  // Print connection details to the Serial Monitor
  Serial.begin(115200);
  Serial.println("Connect to Wi-Fi:");
  Serial.print("SSID: ");
  Serial.println(ssid);
  Serial.print("ESP8266 IP Address: ");
  Serial.println(WiFi.softAPIP());
}

void loop() {
  server.handleClient();
}

void handleChat() {
  String content = "<html><body>";
  content += "<h1>DOORS.message_board</h1>";
  content += "<p>Connect to Wi-Fi:</p>";
  content += "<p>Share something you want, no one will know, or just say something cool, idk...</p>";
  content += "<p>ESP8266 IP Address: " + WiFi.softAPIP().toString() + "</p>";
  content += "<form action='/chat' method='post'>";
  content += "Your name: <input type='text' name='name' value='" + userName + "'><br>"; // Input for choosing the username
  content += "<input type='text' name='message' placeholder='Type your message'>";
  content += "<input type='submit' value='Send'>";
  content += "</form>";
  content += "<h2>Messages:</h2>";
  content += chatLog; // Display messages as they are
  content += "</body></html>";
  server.send(200, "text/html", content);
}

void handleChatMessage() {
  if (server.hasArg("name") && server.hasArg("message")) {
    userName = server.arg("name"); // Update the user's name
    String message = server.arg("message");
    chatLog += userName + ": " + message + "<br>"; // Append the message to the chat log
  }
  server.sendHeader("Location", "/");
  server.send(302, "text/plain", "Message received");
}
