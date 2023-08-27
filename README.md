# newproject
it is a new project for the development of office accounts
const restify = require('restify');
const { BotFrameworkAdapter, MemoryStorage, ConversationState } = require('botbuilder');

// Create server
const server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, () => {
    console.log(`\n${server.name} listening to ${server.url}`);
});

// Create adapter
const adapter = new BotFrameworkAdapter({
    appId: process.env.MICROSOFT_APP_ID,
    appPassword: process.env.MICROSOFT_APP_PASSWORD
});

// Create bot
const bot = new MyBot();

// Listen for incoming requests
server.post('/api/messages', (req, res) => {
    adapter.processActivity(req, res, async (context) => {
        await bot.run(context);
    });
});

class MyBot {
    constructor() {
        this.onMessage(async (context, next) => {
            console.log('Received message:', context.activity.text);
            await context.sendActivity(`You said: ${context.activity.text}`);
            await next();
        });
    }

    async run(context) {
        const dialogContext = await this.dialogs.createContext(context);

        if (context.activity.type === 'message') {
            await dialogContext.continue();
        } else if (context.activity.type === 'conversationUpdate' && context
