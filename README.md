mkdir ai-humanizer-app
cmkdir public
touch public/index.html
touch server.js
touch .env
d ai-humanizer-app
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI to Humanized Content Converter</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">
</head>
<body>
    <div class="container mt-5">
        <h1 class="text-center">AI Humanizer & Plagiarism Checker</h1>
        <form id="codeForm">
            <div class="mb-3">
                <label for="codeInput" class="form-label">Paste AI-generated Content</label>
                <textarea id="codeInput" class="form-control" rows="10" required></textarea>
            </div>
            <button type="submit" class="btn btn-primary">Convert & Analyze</button>
        </form>

        <div id="results" class="mt-4" style="display: none;">
            <h4>Analysis Results:</h4>
            <canvas id="resultChart" width="400" height="200"></canvas>
            <p id="resultText" class="mt-3"></p>
        </div>
    </div>

    <script>
        document.getElementById('codeForm').addEventListener('submit', async (e) => {
            e.preventDefault();
            const code = document.getElementById('codeInput').value;

            const response = await fetch('/analyze', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ code })
            });

            const data = await response.json();

            document.getElementById('results').style.display = 'block';

            const ctx = document.getElementById('resultChart').getContext('2d');
            new Chart(ctx, {
                type: 'pie',
                data: {
                    labels: ['Human-like', 'Still AI-sounding', 'Potentially Plagiarized'],
                    datasets: [{
                        data: [data.human, data.ai, data.plagiarism],
                        backgroundColor: [
                            'rgba(75, 192, 192, 0.6)',
                            'rgba(255, 99, 132, 0.6)',
                            'rgba(255, 206, 86, 0.6)'
                        ],
                        borderWidth: 1
const express = require('express');
const bodyParser = require('body-parser');
const dotenv = require('dotenv');
const { Configuration, OpenAIApi } = require('openai');

dotenv.config();
const app = express();
const port = process.env.PORT || 3000;

app.use(bodyParser.json());
app.use(express.static('public'));

const configuration = new Configuration({
    apiKey: process.env.OPENAI_API_KEY
});
const openai = new OpenAIApi(configuration);

// Simple AI detection simulation
function detectAI(content) {
    const words = content.split(' ');
    const unique = new Set(words);
    const ratio = unique.size / words.length;
    return ratio < 0.4 ? 70 : 30; // Lower diversity suggests AI
}

// Simple plagiarism score simulation
function checkPlagiarism(text) {
    return Math.random() * 30; // Replace with real API later
}

app.post('/analyze', async (req, res) => {
    const { code } = req.body;

    try {
        const aiResponse = await openai.createChatCompletion({
            model: 'gpt-4',
            messages: [
                {
                    role: 'system',
                    content: 'You are a helpful assistant that rewrites AI-generated text to sound more human-like, with natural tone and structure.'
                },
                {
                    role: 'user',
                    content: code
                }
            ]
        });

        const humanized = aiResponse.data.choices[0].message.content;
        const aiScore = detectAI(humanized);
        const humanScore = 100 - aiScore;
        const plagiarismScore = checkPlagiarism(humanized);

        res.json({
            human: humanScore,
            ai: aiScore,
            plagiarism: Math.round(plagiarismScore),
            summary: `Humanized content with an estimated ${humanScore}% human-likeness and ${Math.round(plagiarismScore)}% plagiarism likelihood.`
        });

    } catch (error) {
        console.error('Error:', error.message);
        res.status(500).json({ error: 'AI humanization failed.' });
    }
});

app.listen(port, () => {
    console.log(`✅ Server running on http://localhost:${port}`);
});
OPENAI_API_KEY=your_openai_key_here
npm init -y
npm install express dotenv openai body-parser
node server.js
