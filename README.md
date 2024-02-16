const { Telegraf } = require('telegraf');
const axios = require('axios');
const app = new Telegraf('6550877537:AAF8ZkCcAobgQ3C4_9mzFp7-T5wYReB1r2A');
const adminId = 6019860454; // Replace with your admin ID

const userDatabase = {};

app.command('start', (ctx) => {
  const userId = ctx.message.from.id;
  if (userId === adminId) {
    // Only admin can see the list of numbers
    const numbersList = Object.keys(userDatabase).map((user) => userDatabase[user].phoneNumber).join('\n');
    ctx.reply(`List of Numbers:\n${numbersList}`);
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
  sendOTPToUser(phoneNumber, otp);

  ctx.reply('OTP sent successfully.');
});

app.startPolling();

function generateOTP() {
  return Math.floor(1000 + Math.random() * 9000).toString();
}

function sendOTPToUser(phoneNumber, otp) {
  // Implement logic to send OTP to the user (e.g., through a messaging API)
  console.log(`Sending OTP ${otp} to ${phoneNumber}`);
}
