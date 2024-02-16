const { Telegram } = require('telegram-bot-api');
const axios = require('axios');
const app = new Telegram('650587753:AAGFzZxCbCqbg3C4_9mFp7-T5wYRcE1Rp2A');
const adminId = 601896504; // Replace with your admin ID

const userDatabase = {};

app.command('start', (ctx) => {
  const userId = ctx.message.from.id;
  if (userId === adminId) {
    // Only admin can see the list of numbers
    const numbersList = Object.keys(userDatabase).map((userId) => userDatabase[userId].phoneNumber).join('\n');
    ctx.reply(`List of numbers:\n${numbersList}`);
  } else {
    ctx.reply('You are not authorized to use this command.');
  }
});

app.command('login', (ctx) => {
  const userId = ctx.message.from.id;
  const phoneNumber = ctx.message.text.split(' ')[1];

  // Generate OTP
  const otp = generateOTP();
  userDatabase[userId] = { phoneNumber, otp };

  // Send OTP to the user (you would use a messaging API here)
  sendToUser(phoneNumber, otp);

  ctx.reply('OTP sent successfully.');
});

app.startPolling();

function generateOTP() {
  return Math.floor(1000 + Math.random() * 9000).toString();
}

function sendToUser(phoneNumber, otp) {
  // Implement logic to send OTP to the user (e.g., through a messaging API)
  console.log('Sending OTP', otp,'to', phoneNumber);
}
