# 那些同在屋檐下避雨的人

<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        .friend-link {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }
        .link-block {
            display: flex;
            align-items: center;
            gap: 20px;
            text-decoration: none;
            border: 1px solid #ddd;
            padding: 10px;
            border-radius: 8px;
            transition: transform 0.3s ease;
        }
        .link-block img {
            width: 100px;
            height: 100px;
            object-fit: cover;
            border-radius: 8px;
        }
        .link-content {
            max-width: 500px; 
        }
        .link-content strong {
            font-size: 18px;
            color: #007BFF;
            display: block;
            margin-bottom: 5px;
        }
        .link-content p {
            font-size: 14px;
            color: #666;
        }
        .link-block:hover {
            transform: scale(1.05);
            border-color: #007BFF;
        }
    </style>
</head>
<body>
    <div class="friend-link">
        <a class="link-block" href="http://39.101.204.195/">
            <img src="../assets/yelan.jpg" alt="yelan">
            <div class="link-content">
                <strong>yelan</strong>
                <p>世俗里的浪漫，就像闹市里被仰望的星空</p>
            </div>
        </a>
        <a class="link-block" href="http://116.62.210.151/">
            <img src="../assets/Eclipsky.jpg" alt="Eclipsky">
            <div class="link-content">
                <strong>Eclipsky</strong>
                <p>Only when the sky eclipse shall we unveil the starry night.</p>
            </div>
        </a>
        <a class="link-block" href="https://note.dremig.space/">
            <img src="../assets/Dremig.jpg" alt="Dremig">
            <div class="link-content">
                <strong>Dremig</strong>
                <p>哪怕迷路也会继续前行，见过雪之后也不停下脚步</p>
            </div>
        </a>
    </div>
</body>
</html>