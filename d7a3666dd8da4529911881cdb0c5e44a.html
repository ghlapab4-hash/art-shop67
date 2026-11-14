<?php
declare(strict_types=1);

/**
 * ============================================================================
 * WEERASAK GAME SHOP — Full-Stack Single File (PHP + MySQL + SweetAlert2)
 * ============================================================================
 * วิธีติดตั้ง:
 * 1. แก้ไขค่าเชื่อมต่อฐานข้อมูล MySQL ด้านล่าง (DB_HOST, DB_NAME, DB_USER, DB_PASS)
 * 2. อัปโหลดไฟล์นี้ชื่อ index.php ขึ้นเซิร์ฟเวอร์ (แนะนำวางรูป QR ไว้ที่ assets/qr-truemoney.png และ assets/qr-line.png หรือแก้ลิงก์รูปในระบบหลังบ้าน)
 * 3. เปิดเว็บครั้งแรก ระบบจะสร้างฐานข้อมูล ตาราง และบัญชี Admin ให้อัตโนมัติ:
 *    - Admin Login: admin@weerasak.shop / Admin@1234
 * ============================================================================
 */

// -------------------- 1. ตั้งค่าระบบ (CONFIG) --------------------
const DB_HOST = '127.0.0.1';
const DB_NAME = 'weerasak_game_shop';
const DB_USER = 'root';
const DB_PASS = '';

// (ตัวเลือกเสริมสำหรับตรวจสลิปธนาคารจริงอัตโนมัติผ่าน SlipOK)
// สมัครและนำ Branch ID + API Key มาใส่ที่นี่ หากเว้นว่างระบบจะตรวจจับ Mini-QR + ป้องกันสลิปซ้ำด้วย SHA-256
const SLIPOK_BRANCH_ID = '';
const SLIPOK_API_KEY   = '';

if (session_status() === PHP_SESSION_NONE) {
    session_start();
}

// -------------------- 2. เชื่อมต่อ MySQL & สร้างตารางอัตโนมัติ --------------------
function getPDO(): PDO
{
    static $pdo = null;
    if ($pdo !== null) {
        return $pdo;
    }

    $pdoRoot = new PDO(
        'mysql:host=' . DB_HOST . ';charset=utf8mb4',
        DB_USER,
        DB_PASS,
        [
            PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
            PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
        ]
    );
    $pdoRoot->exec('CREATE DATABASE IF NOT EXISTS `' . DB_NAME . '` CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci');

    $pdo = new PDO(
        'mysql:host=' . DB_HOST . ';dbname=' . DB_NAME . ';charset=utf8mb4',
        DB_USER,
        DB_PASS,
        [
            PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
            PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
        ]
    );

    initSchemaAndSeed($pdo);
    return $pdo;
}

function initSchemaAndSeed(PDO $pdo): void
{
    static $done = false;
    if ($done) {
        return;
    }
    $done = true;

    $pdo->exec("
        CREATE TABLE IF NOT EXISTS `settings` (
            `id` INT PRIMARY KEY DEFAULT 1,
            `site_name` VARCHAR(150) NOT NULL DEFAULT 'WEERASAK GAME SHOP',
            `site_title` VARCHAR(255) NOT NULL DEFAULT 'WEERASAK GAME SHOP — ร้านขายไอเทมเกมและโค้ดเกมออนไลน์',
            `voucher_phone` VARCHAR(20) NOT NULL DEFAULT '0989943840',
            `qr_recipient_name` VARCHAR(150) NOT NULL DEFAULT 'วีรภัทร วงค์แก้ว',
            `qr_image_url` TEXT NOT NULL,
            `line_qr_image_url` TEXT NOT NULL,
            `line_id` VARCHAR(100) NOT NULL DEFAULT '@0969505741',
            `facebook_url` VARCHAR(255) NOT NULL DEFAULT 'https://www.facebook.com/share/1CjKRWwrd5/',
            `updated_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
        ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

        CREATE TABLE IF NOT EXISTS `users` (
            `id` INT AUTO_INCREMENT PRIMARY KEY,
            `name` VARCHAR(120) NOT NULL,
            `email` VARCHAR(190) NOT NULL UNIQUE,
            `password_hash` VARCHAR(255) NOT NULL,
            `role` ENUM('user','admin') NOT NULL DEFAULT 'user',
            `credit` DECIMAL(12,2) NOT NULL DEFAULT 0.00,
            `total_topup` DECIMAL(12,2) NOT NULL DEFAULT 0.00,
            `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

        CREATE TABLE IF NOT EXISTS `slides` (
            `id` INT AUTO_INCREMENT PRIMARY KEY,
            `title` VARCHAR(200) NOT NULL,
            `subtitle` VARCHAR(255) DEFAULT '',
            `image_url` TEXT NOT NULL,
            `link_url` VARCHAR(255) DEFAULT '#products',
            `is_active` TINYINT(1) NOT NULL DEFAULT 1,
            `sort_order` INT NOT NULL DEFAULT 0
        ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

        CREATE TABLE IF NOT EXISTS `categories` (
            `id` INT AUTO_INCREMENT PRIMARY KEY,
            `name` VARCHAR(150) NOT NULL,
            `description` TEXT,
            `image_url` TEXT,
            `show_on_home` TINYINT(1) NOT NULL DEFAULT 1,
            `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

        CREATE TABLE IF NOT EXISTS `products` (
            `id` INT AUTO_INCREMENT PRIMARY KEY,
            `category_id` INT NOT NULL,
            `name` VARCHAR(180) NOT NULL,
            `description` TEXT,
            `price` DECIMAL(12,2) NOT NULL DEFAULT 0.00,
            `image_url` TEXT,
            `show_on_home` TINYINT(1) NOT NULL DEFAULT 1,
            `is_active` TINYINT(1) NOT NULL DEFAULT 1,
            `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
            FOREIGN KEY (`category_id`) REFERENCES `categories`(`id`) ON DELETE CASCADE
        ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

        CREATE TABLE IF NOT EXISTS `stock_items` (
            `id` INT AUTO_INCREMENT PRIMARY KEY,
            `product_id` INT NOT NULL,
            `item_code` TEXT NOT NULL,
            `is_sold` TINYINT(1) NOT NULL DEFAULT 0,
            `sold_to_user_id` INT NULL,
            `sold_at` TIMESTAMP NULL,
            `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
            FOREIGN KEY (`product_id`) REFERENCES `products`(`id`) ON DELETE CASCADE
        ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

        CREATE TABLE IF NOT EXISTS `orders` (
            `id` INT AUTO_INCREMENT PRIMARY KEY,
            `user_id` INT NOT NULL,
            `product_id` INT NOT NULL,
            `product_name` VARCHAR(180) NOT NULL,
            `price` DECIMAL(12,2) NOT NULL,
            `delivered_code` TEXT NOT NULL,
            `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
            FOREIGN KEY (`user_id`) REFERENCES `users`(`id`) ON DELETE CASCADE
        ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

        CREATE TABLE IF NOT EXISTS `topup_logs` (
            `id` INT AUTO_INCREMENT PRIMARY KEY,
            `user_id` INT NOT NULL,
            `method` ENUM('voucher','qr_slip') NOT NULL,
            `amount` DECIMAL(12,2) NOT NULL,
            `reference_code` VARCHAR(190) NOT NULL UNIQUE,
            `slip_sha256` VARCHAR(64) NULL UNIQUE,
            `status` ENUM('approved','rejected') NOT NULL DEFAULT 'approved',
            `detail` VARCHAR(255) DEFAULT '',
            `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
            FOREIGN KEY (`user_id`) REFERENCES `users`(`id`) ON DELETE CASCADE
        ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
    ");

    // เพิ่มคอลัมน์ is_active ถ้ายังไม่มี (สำหรับอัปเกรดจากเวอร์ชันเก่า)
    try { $pdo->exec("ALTER TABLE `products` ADD COLUMN `is_active` TINYINT(1) NOT NULL DEFAULT 1"); } catch (Exception $e) {}

    // Seed Default Settings
    $hasSettings = (int) $pdo->query('SELECT COUNT(*) FROM settings WHERE id = 1')->fetchColumn();
    if ($hasSettings === 0) {
        $stmt = $pdo->prepare('INSERT INTO settings (id, site_name, site_title, voucher_phone, qr_recipient_name, qr_image_url, line_qr_image_url, line_id, facebook_url) VALUES (1, ?, ?, ?, ?, ?, ?, ?, ?)');
        $stmt->execute([
            'WEERASAK GAME SHOP',
            'WEERASAK GAME SHOP — ร้านขายไอเทมเกมและโค้ดเกมออนไลน์',
            '0989943840',
            'วีรภัทร วงค์แก้ว',
            'assets/qr-truemoney.png',
            'assets/qr-line.png',
            '@0969505741',
            'https://www.facebook.com/share/1CjKRWwrd5/',
        ]);
    }

    // Seed Default Admin (admin@weerasak.shop / Admin@1234)
    $hasAdmin = (int) $pdo->query("SELECT COUNT(*) FROM users WHERE role = 'admin'")->fetchColumn();
    if ($hasAdmin === 0) {
        $hash = password_hash('Admin@1234', PASSWORD_DEFAULT);
        $pdo->prepare('INSERT IGNORE INTO users (name, email, password_hash, role, credit, total_topup) VALUES (?, ?, ?, "admin", 9999.00, 10000.00)')
            ->execute(['วีรภัทร (Admin)', 'admin@weerasak.shop', $hash]);
    }

    // Seed Demo Store Data once
    $hasSlides = (int) $pdo->query('SELECT COUNT(*) FROM slides')->fetchColumn();
    $hasCats = (int) $pdo->query('SELECT COUNT(*) FROM categories')->fetchColumn();
    if ($hasSlides === 0) {
        $pdo->prepare('INSERT INTO slides (title, subtitle, image_url, link_url, is_active, sort_order) VALUES (?, ?, ?, ?, 1, 1)')
            ->execute([
                'WEERASAK GAME SHOP — ร้านขายไอเทมเกม 24 ชั่วโมง',
                'ส่งโค้ดเกมและไอดีเกมอัตโนมัติทันทีหลังชำระเงิน ปลอดภัย 100%',
                'https://placehold.co/1300x400/eff6ff/1d4ed8?text=WEERASAK+GAME+SHOP+1300x400',
                '#products',
            ]);
    }
    if ($hasCats === 0) {
        $pdo->prepare('INSERT INTO categories (name, description, image_url, show_on_home) VALUES (?, ?, ?, 1)')
            ->execute(['VALORANT & RIOT', 'ไอดีพร้อมแรงค์และโค้ดเติมพอยต์ราคาพิเศษ', 'https://placehold.co/600x400/f8fafc/0f172a?text=VALORANT', 1]);
        $cat1 = (int) $pdo->lastInsertId();

        $pdo->prepare('INSERT INTO categories (name, description, image_url, show_on_home) VALUES (?, ?, ?, 1)')
            ->execute(['ROBLOX & GAME PASS', 'โค้ด Robux และไอดีเกมยอดนิยม', 'https://placehold.co/600x400/f8fafc/2563eb?text=ROBLOX', 1]);
        $cat2 = (int) $pdo->lastInsertId();

        $pdo->prepare('INSERT INTO categories (name, description, image_url, show_on_home) VALUES (?, ?, ?, 1)')
            ->execute(['STEAM WALLET CODE', 'รหัสบัตรเติมเงิน Steam แท้ส่งไว', 'https://placehold.co/600x400/f8fafc/059669?text=STEAM', 1]);
        $cat3 = (int) $pdo->lastInsertId();

        $pStmt = $pdo->prepare('INSERT INTO products (category_id, name, description, price, image_url, show_on_home) VALUES (?, ?, ?, ?, ?, 1)');
        $pStmt->execute([$cat1, 'Valorant 1,150 VP Code (TH)', 'รหัสเติม VP เซิร์ฟเวอร์ไทย ส่งโค้ดอัตโนมัติทันที', 320.00, 'https://placehold.co/600x450/eff6ff/1e40af?text=VALORANT+VP']);
        $p1 = (int) $pdo->lastInsertId();

        $pStmt->execute([$cat2, 'Roblox Gift Card 800 Robux', 'โค้ดแท้แลกได้ทันทีทั่วโลก ไม่จำกัดโซน', 350.00, 'https://placehold.co/600x450/f0fdf4/166534?text=ROBUX+800']);
        $p2 = (int) $pdo->lastInsertId();

        $pStmt->execute([$cat3, 'Steam Wallet 500 THB', 'โค้ดเติมเงิน Steam ไทย รับรหัสทันที', 500.00, 'https://placehold.co/600x450/f8fafc/334155?text=STEAM+500']);
        $p3 = (int) $pdo->lastInsertId();

        $sStmt = $pdo->prepare('INSERT INTO stock_items (product_id, item_code) VALUES (?, ?)');
        $sStmt->execute([$p1, 'VAL-TH-9821-XKLP-001']);
        $sStmt->execute([$p1, 'VAL-TH-9821-XKLP-002']);
        $sStmt->execute([$p2, 'RBX-800-GIFT-7741-AA']);
        $sStmt->execute([$p3, 'STM-500-CODE-8820-ZZ']);
    }
}

// -------------------- 3. HELPER FUNCTIONS --------------------
function jsonResponse(array $payload, int $status = 200): never
{
    http_response_code($status);
    header('Content-Type: application/json; charset=utf-8');
    echo json_encode($payload, JSON_UNESCAPED_UNICODE);
    exit;
}

function maskPhone(string $phone): string
{
    $digits = preg_replace('/\D/', '', $phone) ?? '';
    if (strlen($digits) < 7) {
        return '***-***-****';
    }
    return substr($digits, 0, 3) . '-XXX-X' . substr($digits, -3);
}

function currentUser(): ?array
{
    if (empty($_SESSION['user_id'])) {
        return null;
    }
    $stmt = getPDO()->prepare('SELECT id, name, email, role, credit, total_topup, created_at FROM users WHERE id = ?');
    $stmt->execute([$_SESSION['user_id']]);
    $user = $stmt->fetch();
    return $user ?: null;
}

function requireAuth(): array
{
    $user = currentUser();
    if (!$user) {
        jsonResponse(['error' => 'กรุณาเข้าสู่ระบบก่อนทำรายการ'], 401);
    }
    return $user;
}

function requireAdmin(): array
{
    $user = requireAuth();
    if (($user['role'] ?? 'user') !== 'admin') {
        jsonResponse(['error' => 'สำหรับผู้ดูแลระบบ (Admin) เท่านั้น'], 403);
    }
    return $user;
}

// -------------------- 4. CLASS VOUCHER (TRUEMONEY ANGPAO) --------------------
class Voucher
{
    private string $mobile;
    private string $voucherUrl;

    public function __construct(string $mobile, string $voucherUrl)
    {
        $this->mobile = preg_replace('/\D/', '', $mobile) ?? '';
        $this->voucherUrl = trim($voucherUrl);
    }

    public function extractCode(): string
    {
        if (preg_match('/gift\.truemoney\.com\/campaign\/?\?v=([A-Za-z0-9]+)/i', $this->voucherUrl, $matches)) {
            return $matches[1];
        }
        if (preg_match('/^[A-Za-z0-9]{12,40}$/', $this->voucherUrl)) {
            return $this->voucherUrl;
        }
        return '';
    }

    public function redeem(): array
    {
        $code = $this->extractCode();
        if ($code === '') {
            return ['status' => 'error', 'message' => 'รูปแบบลิงก์ซองอั่งเปา TrueMoney ไม่ถูกต้อง'];
        }
        if ($this->mobile === '') {
            return ['status' => 'error', 'message' => 'ยังไม่ได้ตั้งค่าเบอร์โทรศัพท์รับเงินในระบบหลังบ้าน'];
        }

        $endpoint = 'https://gift.truemoney.com/campaign/vouchers/' . urlencode($code) . '/redeem';
        $payload = json_encode(['mobile' => $this->mobile, 'voucher_hash' => $code]);

        $ch = curl_init($endpoint);
        curl_setopt_array($ch, [
            CURLOPT_RETURNTRANSFER => true,
            CURLOPT_POST => true,
            CURLOPT_POSTFIELDS => $payload,
            CURLOPT_TIMEOUT => 15,
            CURLOPT_HTTPHEADER => [
                'Content-Type: application/json',
                'Accept: application/json',
                'User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)',
            ],
        ]);
        $responseBody = curl_exec($ch);
        $curlError = curl_error($ch);
        curl_close($ch);

        if ($responseBody === false) {
            return ['status' => 'error', 'message' => 'ไม่สามารถเชื่อมต่อ TrueMoney Gateway: ' . $curlError];
        }

        $data = json_decode($responseBody, true);
        if (($data['status']['code'] ?? '') === 'SUCCESS') {
            return [
                'status' => 'success',
                'amount' => (float) ($data['data']['my_ticket']['amount_baht'] ?? 0),
                'code' => $code,
                'message' => 'เติมเงินซองอั่งเปาสำเร็จ',
            ];
        }
        return ['status' => 'error', 'message' => $data['status']['message'] ?? 'ซองอั่งเปาถูกใช้งานแล้วหรือหมดอายุ'];
    }
}

// -------------------- 5. API ROUTER (?api=...) --------------------
if (isset($_GET['api'])) {
    $pdo = getPDO();
    $api = (string) $_GET['api'];
    $input = json_decode(file_get_contents('php://input') ?: '{}', true) ?? [];

    try {
        switch ($api) {
            // ---------- PUBLIC BOOTSTRAP ----------
            case 'bootstrap':
                $settings = $pdo->query('SELECT * FROM settings WHERE id = 1')->fetch();
                $slides = $pdo->query('SELECT * FROM slides WHERE is_active = 1 ORDER BY sort_order ASC, id DESC')->fetchAll();
                $categories = $pdo->query('SELECT * FROM categories ORDER BY id ASC')->fetchAll();
                $products = $pdo->query('
                    SELECT p.*,
                           (SELECT COUNT(*) FROM stock_items s WHERE s.product_id = p.id AND s.is_sold = 0) AS available_stock,
                           (SELECT COUNT(*) FROM stock_items s WHERE s.product_id = p.id AND s.is_sold = 1) AS sold_count
                    FROM products p WHERE p.is_active = 1 ORDER BY p.id DESC
                ')->fetchAll();

                jsonResponse([
                    'settings' => [
                        'site_name' => $settings['site_name'],
                        'site_title' => $settings['site_title'],
                        'masked_voucher_phone' => maskPhone((string) $settings['voucher_phone']),
                        'qr_recipient_name' => $settings['qr_recipient_name'],
                        'qr_image_url' => $settings['qr_image_url'],
                        'line_qr_image_url' => $settings['line_qr_image_url'],
                        'line_id' => $settings['line_id'],
                        'facebook_url' => $settings['facebook_url'],
                    ],
                    'stats' => [
                        'total_members' => (int) $pdo->query('SELECT COUNT(*) FROM users')->fetchColumn(),
                        'total_stock' => (int) $pdo->query('SELECT COUNT(*) FROM stock_items WHERE is_sold = 0')->fetchColumn(),
                        'total_sold' => (int) $pdo->query('SELECT COUNT(*) FROM orders')->fetchColumn(),
                    ],
                    'slides' => $slides,
                    'categories' => $categories,
                    'products' => $products,
                    'user' => currentUser(),
                ]);

            // ---------- AUTH ----------
            case 'register':
                $name = trim((string) ($input['name'] ?? ''));
                $email = strtolower(trim((string) ($input['email'] ?? '')));
                $password = (string) ($input['password'] ?? '');
                $confirm = (string) ($input['confirm_password'] ?? '');

                if (mb_strlen($name) < 2) jsonResponse(['error' => 'กรุณากรอกชื่อผู้ใช้งานอย่างน้อย 2 ตัวอักษร'], 400);
                if (!filter_var($email, FILTER_VALIDATE_EMAIL)) jsonResponse(['error' => 'รูปแบบอีเมลไม่ถูกต้อง'], 400);
                if (strlen($password) < 6) jsonResponse(['error' => 'รหัสผ่านต้องมีอย่างน้อย 6 ตัวอักษร'], 400);
                if ($password !== $confirm) jsonResponse(['error' => 'รหัสผ่านและยืนยันรหัสผ่านไม่ตรงกัน'], 400);

                $exists = $pdo->prepare('SELECT id FROM users WHERE email = ?');
                $exists->execute([$email]);
                if ($exists->fetch()) jsonResponse(['error' => 'อีเมลนี้ถูกใช้งานแล้ว กรุณาใช้อีเมลอื่น'], 409);

                $hash = password_hash($password, PASSWORD_DEFAULT);
                $pdo->prepare('INSERT INTO users (name, email, password_hash, role, credit, total_topup) VALUES (?, ?, ?, "user", 0, 0)')
                    ->execute([$name, $email, $hash]);
                $_SESSION['user_id'] = (int) $pdo->lastInsertId();
                jsonResponse(['user' => currentUser()], 201);

            case 'login':
                $email = strtolower(trim((string) ($input['email'] ?? '')));
                $password = (string) ($input['password'] ?? '');
                $stmt = $pdo->prepare('SELECT * FROM users WHERE email = ? OR name = ? LIMIT 1');
                $stmt->execute([$email, $email]);
                $user = $stmt->fetch();
                if (!$user || !password_verify($password, $user['password_hash'])) {
                    jsonResponse(['error' => 'อีเมลหรือรหัสผ่านไม่ถูกต้อง'], 401);
                }
                $_SESSION['user_id'] = (int) $user['id'];
                jsonResponse(['user' => currentUser()]);

            case 'logout':
                session_destroy();
                jsonResponse(['ok' => true]);

            // ---------- BUY & HISTORY ----------
            case 'buy':
                $user = requireAuth();
                $productId = (int) ($input['product_id'] ?? 0);
                $pdo->beginTransaction();
                try {
                    $pStmt = $pdo->prepare('SELECT * FROM products WHERE id = ? LIMIT 1');
                    $pStmt->execute([$productId]);
                    $product = $pStmt->fetch();
                    if (!$product) throw new RuntimeException('ไม่พบสินค้าที่ต้องการสั่งซื้อ');
                    if (!$product['is_active']) throw new RuntimeException('สินค้านี้ถูกปิดการขายชั่วคราว');

                    $uStmt = $pdo->prepare('SELECT * FROM users WHERE id = ? FOR UPDATE');
                    $uStmt->execute([$user['id']]);
                    $freshUser = $uStmt->fetch();
                    if ((float) $freshUser['credit'] < (float) $product['price']) {
                        throw new RuntimeException('เครดิตคงเหลือไม่เพียงพอ กรุณาเติมเงินก่อนทำรายการ');
                    }

                    $sStmt = $pdo->prepare('SELECT * FROM stock_items WHERE product_id = ? AND is_sold = 0 ORDER BY id ASC LIMIT 1 FOR UPDATE');
                    $sStmt->execute();
                    $stock = $sStmt->fetch();
                    if (!$stock) throw new RuntimeException('สินค้าหมดสต็อก กรุณาติดต่อแอดมินเพื่อเติมสต็อก');

                    // หักเครดิต
                    $pdo->prepare('UPDATE users SET credit = credit - ? WHERE id = ?')
                        ->execute([$product['price'], $user['id']]);
                    // ทำเครื่องหมายโค้ดว่าถูกขายแล้ว
                    $pdo->prepare('UPDATE stock_items SET is_sold = 1, sold_to_user_id = ?, sold_at = NOW() WHERE id = ?')
                        ->execute([$user['id'], $stock['id']]);
                    // บันทึกออเดอร์
                    $pdo->prepare('INSERT INTO orders (user_id, product_id, product_name, price, delivered_code) VALUES (?, ?, ?, ?, ?)')
                        ->execute([$user['id'], $productId, $product['name'], $product['price'], $stock['item_code']]);

                    $pdo->commit();
                    jsonResponse([
                        'success' => true,
                        'product_name' => $product['name'],
                        'price' => (float) $product['price'],
                        'code' => $stock['item_code'],
                        'credit_remaining' => (float) $freshUser['credit'] - (float) $product['price'],
                    ]);
                } catch (Throwable $e) {
                    $pdo->rollBack();
                    throw $e;
                }

            case 'history':
                $user = requireAuth();
                $stmt = $pdo->prepare('SELECT * FROM orders WHERE user_id = ? ORDER BY id DESC LIMIT 50');
                $stmt->execute([$user['id']]);
                jsonResponse(['orders' => $stmt->fetchAll()]);

            // ---------- TOPUP ----------
            case 'topup_voucher':
                $user = requireAuth();
                $voucherUrl = trim((string) ($input['voucher_url'] ?? ''));
                $settings = $pdo->query('SELECT * FROM settings WHERE id = 1')->fetch();
                $voucher = new Voucher((string) $settings['voucher_phone'], $voucherUrl);
                $result = $voucher->redeem();

                if ($result['status'] !== 'success') {
                    jsonResponse(['error' => $result['message']], 400);
                }
                $amount = (float) $result['amount'];
                if ($amount <= 0) jsonResponse(['error' => 'ไม่สามารถอ่านจำนวนเงินจากซองอั่งเปาได้'], 400);

                $ref = 'VCH-' . $result['code'];
                try {
                    $pdo->prepare('INSERT INTO topup_logs (user_id, method, amount, reference_code, status, detail) VALUES (?, "voucher", ?, ?, "approved", ?)')
                        ->execute([$user['id'], $amount, $ref, 'ซองอั่งเปา TrueMoney']);
                } catch (PDOException $e) {
                    jsonResponse(['error' => 'ซองอั่งเปานี้เคยถูกใช้งานในระบบแล้ว'], 409);
                }
                $pdo->prepare('UPDATE users SET credit = credit + ?, total_topup = total_topup + ? WHERE id = ?')
                    ->execute([$amount, $amount, $user['id']]);
                jsonResponse(['success' => true, 'amount' => $amount, 'message' => 'เติมเงินสำเร็จ ' . $amount . ' บาท']);

            case 'topup_qr':
                $user = requireAuth();
                $amount = (float) ($input['amount'] ?? 0);
                $reference = trim((string) ($input['reference_code'] ?? ''));
                $slipSha = trim((string) ($input['slip_sha256'] ?? ''));
                if ($amount <= 0) jsonResponse(['error' => 'กรุณาระบุจำนวนเงินที่ถูกต้อง'], 400);
                if ($reference === '') jsonResponse(['error' => 'กรุณาระบุรหัสอ้างอิงการโอน'], 400);

                // ตรวจสลิปซ้ำด้วย SHA-256
                if ($slipSha !== '') {
                    $dup = $pdo->prepare('SELECT id FROM topup_logs WHERE slip_sha256 = ?');
                    $dup->execute([$slipSha]);
                    if ($dup->fetch()) jsonResponse(['error' => 'สลิปนี้เคยถูกใช้งานแล้ว (ตรวจพบสลิปซ้ำ)'], 409);
                }
                try {
                    $pdo->prepare('INSERT INTO topup_logs (user_id, method, amount, reference_code, slip_sha256, status, detail) VALUES (?, "qr_slip", ?, ?, ?, "approved", "โอนผ่าน QR PromptPay")')
                        ->execute([$user['id'], $amount, $reference, $slipSha !== '' ? $slipSha : null]);
                } catch (PDOException $e) {
                    jsonResponse(['error' => 'รหัสอ้างอิงนี้เคยถูกใช้งานแล้ว'], 409);
                }
                $pdo->prepare('UPDATE users SET credit = credit + ?, total_topup = total_topup + ? WHERE id = ?')
                    ->execute([$amount, $amount, $user['id']]);
                jsonResponse(['success' => true, 'amount' => $amount, 'message' => 'แจ้งโอนสำเร็จ เครดิตถูกเพิ่ม ' . $amount . ' บาท']);

            // ---------- ADMIN APIs ----------
            case 'admin_stats':
                requireAdmin();
                jsonResponse([
                    'total_members' => (int) $pdo->query('SELECT COUNT(*) FROM users')->fetchColumn(),
                    'total_orders' => (int) $pdo->query('SELECT COUNT(*) FROM orders')->fetchColumn(),
                    'total_revenue' => (float) $pdo->query('SELECT COALESCE(SUM(price),0) FROM orders')->fetchColumn(),
                    'total_topup' => (float) $pdo->query('SELECT COALESCE(SUM(amount),0) FROM topup_logs WHERE status="approved"')->fetchColumn(),
                    'total_stock' => (int) $pdo->query('SELECT COUNT(*) FROM stock_items WHERE is_sold=0')->fetchColumn(),
                    'low_stock' => $pdo->query('SELECT p.id, p.name, (SELECT COUNT(*) FROM stock_items s WHERE s.product_id=p.id AND s.is_sold=0) AS stock FROM products p HAVING stock < 3 ORDER BY stock ASC')->fetchAll(),
                ]);

            case 'settings_update':
                requireAdmin();
                $fields = ['site_name','site_title','voucher_phone','qr_recipient_name','qr_image_url','line_qr_image_url','line_id','facebook_url'];
                $sets = []; $vals = [];
                foreach ($fields as $f) { if (isset($input[$f])) { $sets[] = "`$f` = ?"; $vals[] = (string) $input[$f]; } }
                if ($sets) {
                    $vals[] = 1;
                    $pdo->prepare('UPDATE settings SET ' . implode(',', $sets) . ' WHERE id = ?')->execute($vals);
                }
                jsonResponse(['success' => true, 'settings' => $pdo->query('SELECT * FROM settings WHERE id=1')->fetch()]);

            case 'slides_manage':
                requireAdmin();
                $action = (string) ($input['action'] ?? 'list');
                if ($action === 'list') jsonResponse(['slides' => $pdo->query('SELECT * FROM slides ORDER BY sort_order ASC, id DESC')->fetchAll()]);
                if ($action === 'add') {
                    $pdo->prepare('INSERT INTO slides (title, subtitle, image_url, link_url, is_active, sort_order) VALUES (?,?,?,?,?,?)')
                        ->execute([$input['title']??'', $input['subtitle']??'', $input['image_url']??'', $input['link_url']??'#products', (int)($input['is_active']??1), (int)($input['sort_order']??0)]);
                    jsonResponse(['success'=>true,'id'=>(int)$pdo->lastInsertId()]);
                }
                if ($action === 'delete') {
                    $pdo->prepare('DELETE FROM slides WHERE id = ?')->execute([(int)($input['id']??0)]);
                    jsonResponse(['success'=>true]);
                }
                jsonResponse(['error'=>'action ไม่ถูกต้อง'],400);

            case 'categories_manage':
                requireAdmin();
                $action = (string) ($input['action'] ?? 'list');
                if ($action === 'list') jsonResponse(['categories' => $pdo->query('SELECT * FROM categories ORDER BY id ASC')->fetchAll()]);
                if ($action === 'add') {
                    $pdo->prepare('INSERT INTO categories (name, description, image_url, show_on_home) VALUES (?,?,?,?)')
                        ->execute([$input['name']??'', $input['description']??'', $input['image_url']??'', (int)($input['show_on_home']??1)]);
                    jsonResponse(['success'=>true,'id'=>(int)$pdo->lastInsertId()]);
                }
                if ($action === 'update') {
                    $pdo->prepare('UPDATE categories SET name=?, description=?, image_url=?, show_on_home=? WHERE id=?')
                        ->execute([$input['name']??'', $input['description']??'', $input['image_url']??'', (int)($input['show_on_home']??1), (int)($input['id']??0)]);
                    jsonResponse(['success'=>true]);
                }
                if ($action === 'delete') {
                    $pdo->prepare('DELETE FROM categories WHERE id = ?')->execute([(int)($input['id']??0)]);
                    jsonResponse(['success'=>true]);
                }
                jsonResponse(['error'=>'action ไม่ถูกต้อง'],400);

            case 'products_manage':
                requireAdmin();
                $action = (string) ($input['action'] ?? 'list');
                if ($action === 'list') jsonResponse(['products' => $pdo->query('SELECT p.*, c.name AS category_name, (SELECT COUNT(*) FROM stock_items s WHERE s.product_id=p.id AND s.is_sold=0) AS stock FROM products p LEFT JOIN categories c ON c.id=p.category_id ORDER BY p.id DESC')->fetchAll()]);
                if ($action === 'add') {
                    $pdo->prepare('INSERT INTO products (category_id, name, description, price, image_url, show_on_home, is_active) VALUES (?,?,?,?,?,?,1)')
                        ->execute([(int)($input['category_id']??0), $input['name']??'', $input['description']??'', (float)($input['price']??0), $input['image_url']??'', (int)($input['show_on_home']??1)]);
                    jsonResponse(['success'=>true,'id'=>(int)$pdo->lastInsertId()]);
                }
                if ($action === 'update') {
                    $pdo->prepare('UPDATE products SET category_id=?, name=?, description=?, price=?, image_url=?, show_on_home=?, is_active=? WHERE id=?')
                        ->execute([(int)($input['category_id']??0), $input['name']??'', $input['description']??'', (float)($input['price']??0), $input['image_url']??'', (int)($input['show_on_home']??1), (int)($input['is_active']??1), (int)($input['id']??0)]);
                    jsonResponse(['success'=>true]);
                }
                if ($action === 'delete') {
                    $pdo->prepare('DELETE FROM products WHERE id = ?')->execute([(int)($input['id']??0)]);
                    jsonResponse(['success'=>true]);
                }
                jsonResponse(['error'=>'action ไม่ถูกต้อง'],400);

            case 'stock_manage':
                requireAdmin();
                $action = (string) ($input['action'] ?? 'list');
                if ($action === 'list') {
                    $pid = (int)($input['product_id']??0);
                    $stmt = $pdo->prepare('SELECT * FROM stock_items WHERE product_id = ? ORDER BY id DESC LIMIT 200');
                    $stmt->execute([$pid]);
                    jsonResponse(['items' => $stmt->fetchAll()]);
                }
                if ($action === 'add') {
                    $pid = (int)($input['product_id']??0);
                    $codes = preg_split('/\r\n|\r|\n/', trim((string)($input['codes']??'')));
                    $added = 0;
                    $stmt = $pdo->prepare('INSERT INTO stock_items (product_id, item_code) VALUES (?, ?)');
                    foreach ($codes as $c) { $c = trim($c); if ($c !== '') { $stmt->execute([$pid, $c]); $added++; } }
                    jsonResponse(['success'=>true,'added'=>$added]);
                }
                if ($action === 'delete') {
                    $pdo->prepare('DELETE FROM stock_items WHERE id = ?')->execute([(int)($input['id']??0)]);
                    jsonResponse(['success'=>true]);
                }
                jsonResponse(['error'=>'action ไม่ถูกต้อง'],400);

            case 'users_manage':
                requireAdmin();
                $action = (string) ($input['action'] ?? 'list');
                if ($action === 'list') jsonResponse(['users' => $pdo->query('SELECT id, name, email, role, credit, total_topup, created_at FROM users ORDER BY id DESC')->fetchAll()]);
                if ($action === 'update') {
                    $uid = (int)($input['id']??0);
                    if (isset($input['role'])) $pdo->prepare('UPDATE users SET role=? WHERE id=?')->execute([$input['role']==='admin'?'admin':'user', $uid]);
                    if (isset($input['credit_delta'])) $pdo->prepare('UPDATE users SET credit = credit + ? WHERE id=?')->execute([(float)$input['credit_delta'], $uid]);
                    if (!empty($input['new_password'])) {
                        $hash = password_hash((string)$input['new_password'], PASSWORD_DEFAULT);
                        $pdo->prepare('UPDATE users SET password_hash=? WHERE id=?')->execute([$hash, $uid]);
                    }
                    jsonResponse(['success'=>true]);
                }
                if ($action === 'delete') {
                    $pdo->prepare('DELETE FROM users WHERE id = ? AND role != "admin"')->execute([(int)($input['id']??0)]);
                    jsonResponse(['success'=>true]);
                }
                jsonResponse(['error'=>'action ไม่ถูกต้อง'],400);

            case 'topup_logs':
                requireAdmin();
                jsonResponse(['logs' => $pdo->query('SELECT t.*, u.name AS user_name FROM topup_logs t LEFT JOIN users u ON u.id=t.user_id ORDER BY t.id DESC LIMIT 200')->fetchAll()]);

            default:
                jsonResponse(['error' => 'API ไม่ถูกต้อง'], 404);
        }
    } catch (Throwable $e) {
        jsonResponse(['error' => $e->getMessage()], 500);
    }
    exit;
}

// -------------------- 6. หน้าเว็บ (HTML + CSS + JS) — White Theme + SweetAlert2 --------------------
$settings = getPDO()->query('SELECT * FROM settings WHERE id = 1')->fetch();
?>
<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title><?= htmlspecialchars($settings['site_title']) ?></title>
<link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
<style>
*{margin:0;padding:0;box-sizing:border-box}
:root{--p:#4f46e5;--pl:#6366f1;--a:#06b6d4;--bg:#f8fafc;--card:#fff;--ink:#1e293b;--m:#64748b;--bd:#e2e8f0;--ok:#10b981;--no:#ef4444;--gold:#f59e0b;--sh:0 4px 20px rgba(0,0,0,.08)}
body{font-family:'Kanit',sans-serif;background:var(--bg);color:var(--ink);line-height:1.6}
a{text-decoration:none;color:inherit}
img{max-width:100%}
.container{max-width:1200px;margin:0 auto;padding:0 16px}

/* ===== NAV ===== */
nav{background:#fff;box-shadow:var(--sh);position:sticky;top:0;z-index:100;border-bottom:1px solid var(--bd)}
.nav-inner{max-width:1200px;margin:0 auto;padding:12px 16px;display:flex;align-items:center;justify-content:space-between;gap:12px}
.logo{font-size:20px;font-weight:800;background:linear-gradient(135deg,var(--p),var(--a));-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text}
.nav-menu{display:flex;gap:6px;align-items:center}
.nav-menu a{padding:8px 14px;border-radius:10px;font-weight:500;font-size:14px;color:var(--m);transition:.2s}
.nav-menu a:hover,.nav-menu a.active{background:#eef2ff;color:var(--p)}
.auth-area{display:flex;align-items:center;gap:8px}
.btn{border:none;border-radius:10px;padding:9px 18px;font-family:inherit;font-weight:600;font-size:14px;cursor:pointer;transition:.2s;display:inline-flex;align-items:center;gap:6px}
.btn-primary{background:linear-gradient(135deg,var(--p),var(--pl));color:#fff;box-shadow:0 4px 14px rgba(79,70,229,.35)}
.btn-primary:hover{transform:translateY(-1px);box-shadow:0 6px 18px rgba(79,70,229,.45)}
.btn-outline{background:#fff;color:var(--p);border:1.5px solid var(--p)}
.btn-outline:hover{background:#eef2ff}
.profile-wrap{position:relative}
.profile-avatar{width:40px;height:40px;border-radius:50%;background:linear-gradient(135deg,var(--p),var(--a));color:#fff;display:flex;align-items:center;justify-content:center;font-weight:700;cursor:pointer;font-size:16px;border:2px solid #fff;box-shadow:var(--sh)}
.dropdown{position:absolute;right:0;top:52px;background:#fff;border-radius:14px;box-shadow:0 10px 40px rgba(0,0,0,.15);border:1px solid var(--bd);min-width:220px;overflow:hidden;display:none;z-index:200}
.dropdown.show{display:block;animation:dropIn .2s ease}
@keyframes dropIn{from{opacity:0;transform:translateY(-8px)}to{opacity:1;transform:translateY(0)}}
.dropdown .dd-head{padding:12px 16px;background:linear-gradient(135deg,#eef2ff,#ecfeff);border-bottom:1px solid var(--bd)}
.dropdown .dd-head .dd-name{font-weight:700;font-size:14px}
.dropdown .dd-head .dd-credit{font-size:12px;color:var(--gold);font-weight:600;margin-top:2px}
.dropdown a,.dropdown button{display:flex;align-items:center;gap:10px;padding:11px 16px;font-size:13.5px;font-weight:500;color:var(--ink);width:100%;text-align:left;background:none;border:none;cursor:pointer;font-family:inherit;transition:.15s}
.dropdown a:hover,.dropdown button:hover{background:#f1f5f9;color:var(--p)}
.dropdown .divider{height:1px;background:var(--bd);margin:4px 0}

/* ===== SLIDER ===== */
.slider{position:relative;max-width:1300px;margin:20px auto 0;border-radius:18px;overflow:hidden;box-shadow:var(--sh);aspect-ratio:1300/400;background:#e2e8f0}
.slide{position:absolute;inset:0;opacity:0;transition:opacity .8s}
.slide.active{opacity:1}
.slide img{width:100%;height:100%;object-fit:cover}
.slide-caption{position:absolute;left:40px;top:50%;transform:translateY(-50%);color:#fff;text-shadow:0 2px 10px rgba(0,0,0,.5);max-width:60%}
.slide-caption h2{font-size:32px;font-weight:800;margin-bottom:8px}
.slide-caption p{font-size:15px;opacity:.95}
.slider-arrow{position:absolute;top:50%;transform:translateY(-50%);width:42px;height:42px;border-radius:50%;background:rgba(255,255,255,.85);border:none;cursor:pointer;font-size:16px;color:var(--ink);z-index:5;transition:.2s}
.slider-arrow:hover{background:#fff;transform:translateY(-50%) scale(1.1)}
.slider-arrow.prev{left:14px}.slider-arrow.next{right:14px}
.slider-dots{position:absolute;bottom:14px;left:50%;transform:translateX(-50%);display:flex;gap:6px;z-index:5}
.slider-dots span{width:9px;height:9px;border-radius:50%;background:rgba(255,255,255,.5);cursor:pointer;transition:.2s}
.slider-dots span.active{background:#fff;width:24px;border-radius:5px}

/* ===== STATS ===== */
.stats-row{display:grid;grid-template-columns:repeat(3,1fr);gap:14px;margin:24px auto;max-width:1300px}
.stat-card{background:#fff;border-radius:16px;padding:20px;display:flex;align-items:center;gap:14px;box-shadow:var(--sh);border:1px solid var(--bd);transition:.2s}
.stat-card:hover{transform:translateY(-3px);box-shadow:0 8px 28px rgba(0,0,0,.12)}
.stat-icon{width:52px;height:52px;border-radius:14px;display:flex;align-items:center;justify-content:center;font-size:24px;color:#fff;flex-shrink:0}
.stat-icon.s1{background:linear-gradient(135deg,#6366f1,#4f46e5)}
.stat-icon.s2{background:linear-gradient(135deg,#06b6d4,#0891b2)}
.stat-icon.s3{background:linear-gradient(135deg,#10b981,#059669)}
.stat-info .num{font-size:26px;font-weight:800;line-height:1.1}
.stat-info .lbl{font-size:13px;color:var(--m);font-weight:500}

/* ===== SECTION ===== */
.section{max-width:1300px;margin:36px auto;padding:0 16px}
.section-title{font-size:22px;font-weight:800;margin-bottom:16px;display:flex;align-items:center;gap:10px}
.section-title::before{content:'';width:5px;height:26px;background:linear-gradient(180deg,var(--p),var(--a));border-radius:3px}

/* ===== CATEGORIES ===== */
.cat-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(170px,1fr));gap:14px}
.cat-card{background:#fff;border-radius:16px;padding:20px 14px;text-align:center;cursor:pointer;box-shadow:var(--sh);border:1px solid var(--bd);transition:.25s;overflow:hidden}
.cat-card:hover{transform:translateY(-4px);box-shadow:0 10px 30px rgba(0,0,0,.14);border-color:var(--pl)}
.cat-icon{font-size:38px;margin-bottom:8px}
.cat-name{font-weight:700;font-size:14px}
.cat-desc{font-size:11.5px;color:var(--m);margin-top:4px;line-height:1.4}

/* ===== PRODUCTS ===== */
.prod-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(240px,1fr));gap:16px}
.prod-card{background:#fff;border-radius:18px;overflow:hidden;box-shadow:var(--sh);border:1px solid var(--bd);transition:.25s;display:flex;flex-direction:column}
.prod-card:hover{transform:translateY(-4px);box-shadow:0 12px 32px rgba(0,0,0,.14)}
.prod-img{aspect-ratio:4/3;background:#f1f5f9;display:flex;align-items:center;justify-content:center;font-size:48px;overflow:hidden;position:relative}
.prod-img img{width:100%;height:100%;object-fit:cover}
.prod-badge{position:absolute;top:10px;left:10px;background:var(--ok);color:#fff;font-size:11px;font-weight:700;padding:3px 10px;border-radius:20px}
.prod-badge.out{background:var(--no)}
.prod-body{padding:14px;flex:1;display:flex;flex-direction:column}
.prod-cat{font-size:11px;color:var(--a);font-weight:600;text-transform:uppercase;letter-spacing:.5px}
.prod-name{font-size:15px;font-weight:700;margin:4px 0 6px;line-height:1.35}
.prod-desc{font-size:12px;color:var(--m);margin-bottom:10px;flex:1}
.prod-stock{font-size:11.5px;color:var(--m);margin-bottom:8px}
.prod-stock b{color:var(--ok)}
.prod-foot{display:flex;align-items:center;justify-content:space-between;gap:8px}
.prod-price{font-size:19px;font-weight:800;color:var(--p)}
.btn-buy{background:linear-gradient(135deg,var(--p),var(--pl));color:#fff;border:none;border-radius:10px;padding:8px 16px;font-family:inherit;font-weight:700;font-size:13px;cursor:pointer;transition:.2s;display:flex;align-items:center;gap:5px}
.btn-buy:hover{transform:scale(1.05);box-shadow:0 4px 14px rgba(79,70,229,.4)}
.btn-buy:disabled{background:#cbd5e1;cursor:not-allowed;transform:none;box-shadow:none}

/* ===== MODAL (Login/Register/Topup) ===== */
.modal-overlay{position:fixed;inset:0;background:rgba(15,23,42,.55);z-index:500;display:none;align-items:center;justify-content:center;padding:16px;backdrop-filter:blur(4px)}
.modal-overlay.show{display:flex;animation:fadeIn .2s}
@keyframes fadeIn{from{opacity:0}to{opacity:1}}
.modal-box{background:#fff;border-radius:20px;padding:28px 24px;width:100%;max-width:400px;box-shadow:0 20px 60px rgba(0,0,0,.3);animation:modalIn .3s cubic-bezier(.34,1.56,.64,1)}
@keyframes modalIn{from{opacity:0;transform:scale(.85) translateY(20px)}to{opacity:1;transform:scale(1) translateY(0)}}
.modal-box h3{font-size:20px;font-weight:800;margin-bottom:4px;text-align:center}
.modal-box .sub{text-align:center;font-size:13px;color:var(--m);margin-bottom:18px}
.form-group{margin-bottom:12px}
.form-group label{display:block;font-size:12.5px;font-weight:600;color:var(--m);margin-bottom:5px}
.form-group input,.form-group textarea,.form-group select{width:100%;padding:11px 14px;border:1.5px solid var(--bd);border-radius:10px;font-family:inherit;font-size:14px;transition:.2s;background:#f8fafc}
.form-group input:focus,.form-group textarea:focus,.form-group select:focus{outline:none;border-color:var(--pl);background:#fff;box-shadow:0 0 0 3px rgba(99,102,241,.15)}
.modal-switch{text-align:center;margin-top:14px;font-size:13px;color:var(--m)}
.modal-switch a{color:var(--p);font-weight:600;cursor:pointer}
.tabs{display:flex;gap:6px;margin-bottom:16px;background:#f1f5f9;padding:5px;border-radius:12px}
.tab{flex:1;padding:9px;border-radius:9px;border:none;background:transparent;font-family:inherit;font-weight:600;font-size:13px;cursor:pointer;color:var(--m);transition:.2s}
.tab.active{background:#fff;color:var(--p);box-shadow:0 2px 8px rgba(0,0,0,.08)}
.tab-content{display:none}.tab-content.active{display:block}
.qr-box{text-align:center;padding:14px;background:#f8fafc;border-radius:14px;margin-bottom:14px}
.qr-box img{max-width:180px;border-radius:12px;border:4px solid #fff;box-shadow:var(--sh)}
.qr-box .qr-name{font-size:13px;font-weight:600;margin-top:8px}
.qr-box .qr-phone{font-size:12px;color:var(--m)}

/* ===== HISTORY ===== */
.history-item{background:#f8fafc;border-radius:12px;padding:12px 14px;margin-bottom:8px;display:flex;align-items:center;justify-content:space-between;gap:10px}
.history-item .h-name{font-weight:600;font-size:13.5px}
.history-item .h-date{font-size:11px;color:var(--m)}
.history-item .h-code{font-family:monospace;font-size:12px;background:#eef2ff;color:var(--p);padding:3px 8px;border-radius:6px;word-break:break-all}
.copy-btn{background:none;border:none;color:var(--p);cursor:pointer;font-size:14px;padding:4px}

/* ===== ADMIN DASHBOARD ===== */
.admin-panel{max-width:1300px;margin:20px auto;padding:0 16px;display:none}
.admin-panel.show{display:block}
.admin-tabs{display:flex;gap:6px;flex-wrap:wrap;margin-bottom:18px}
.admin-tab{padding:9px 16px;border-radius:10px;background:#fff;border:1px solid var(--bd);font-family:inherit;font-weight:600;font-size:13px;cursor:pointer;color:var(--m);transition:.2s}
.admin-tab.active{background:var(--p);color:#fff;border-color:var(--p)}
.admin-section{display:none;background:#fff;border-radius:16px;padding:20px;box-shadow:var(--sh);border:1px solid var(--bd)}
.admin-section.active{display:block}
.admin-section h4{font-size:16px;font-weight:700;margin-bottom:14px}
.admin-table{width:100%;border-collapse:collapse;font-size:13px}
.admin-table th{background:#f1f5f9;padding:10px;text-align:left;font-weight:600;font-size:12px;color:var(--m)}
.admin-table td{padding:10px;border-top:1px solid var(--bd)}
.admin-table tr:hover td{background:#f8fafc}
.btn-sm{padding:5px 10px;border-radius:8px;border:none;font-family:inherit;font-size:12px;font-weight:600;cursor:pointer;margin:2px}
.btn-sm.danger{background:#fee2e2;color:#dc2626}
.btn-sm.warn{background:#fef3c7;color:#d97706}
.btn-sm.info{background:#dbeafe;color:#2563eb}
.admin-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:12px;margin-bottom:16px}
.admin-stat{background:linear-gradient(135deg,#eef2ff,#ecfeff);border-radius:14px;padding:16px;border:1px solid #c7d2fe}
.admin-stat .n{font-size:24px;font-weight:800;color:var(--p)}
.admin-stat .l{font-size:12px;color:var(--m);font-weight:500}
.inline-form{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:14px;align-items:flex-end}
.inline-form .form-group{flex:1;min-width:140px;margin-bottom:0}

/* ===== FLOATING CONTACT ===== */
.float-contact{position:fixed;bottom:24px;right:24px;z-index:90;display:flex;flex-direction:column;gap:10px;align-items:flex-end}
.float-btn{width:54px;height:54px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:24px;color:#fff;cursor:pointer;box-shadow:0 6px 20px rgba(0,0,0,.25);transition:.25s;border:none}
.float-btn:hover{transform:scale(1.1)}
.float-btn.fb{background:linear-gradient(135deg,#1877f2,#0d47a1)}
.float-btn.line{background:linear-gradient(135deg,#06c755,#048a3d)}
.float-qr{position:absolute;bottom:70px;right:0;background:#fff;padding:12px;border-radius:14px;box-shadow:0 10px 40px rgba(0,0,0,.2);display:none;text-align:center;width:180px}
.float-qr.show{display:block;animation:dropIn .2s}
.float-qr img{width:140px;border-radius:10px}
.float-qr p{font-size:11px;color:var(--m);margin-top:6px}

/* ===== FOOTER ===== */
footer{background:#fff;border-top:1px solid var(--bd);margin-top:50px;padding:28px 16px;text-align:center;color:var(--m);font-size:13px}
footer b{color:var(--ink)}

@media(max-width:640px){
  .nav-menu{display:none}
  .slide-caption{left:20px;max-width:80%}
  .slide-caption h2{font-size:20px}
  .slide-caption p{font-size:12px}
  .stats-row{grid-template-columns:1fr}
  .stat-card{padding:14px}
  .section-title{font-size:18px}
  .prod-grid{grid-template-columns:repeat(2,1fr);gap:10px}
  .prod-name{font-size:13px}
  .prod-price{font-size:16px}
  .btn-buy{padding:7px 10px;font-size:12px}
}
</style>
</head>
<body>

<!-- NAV -->
<nav>
  <div class="nav-inner">
    <a href="#" class="logo" id="siteName"><?= htmlspecialchars($settings['site_name']) ?></a>
    <div class="nav-menu">
      <a href="#home" class="active"><i class="fas fa-home"></i> หน้าหลัก</a>
      <a href="#categories"><i class="fas fa-th-large"></i> หมวดหมู่</a>
      <a href="#products"><i class="fas fa-box"></i> สินค้า</a>
      <a href="#" onclick="openTopup();return false;"><i class="fas fa-wallet"></i> เติมเงิน</a>
    </div>
    <div class="auth-area" id="authArea">
      <button class="btn btn-outline" onclick="openAuth('login')">เข้าสู่ระบบ</button>
      <button class="btn btn-primary" onclick="openAuth('register')">สมัครสมาชิก</button>
    </div>
  </div>
</nav>

<!-- SLIDER -->
<div class="slider" id="home">
  <div id="slidesWrap"></div>
  <button class="slider-arrow prev" onclick="slideMove(-1)"><i class="fas fa-chevron-left"></i></button>
  <button class="slider-arrow next" onclick="slideMove(1)"><i class="fas fa-chevron-right"></i></button>
  <div class="slider-dots" id="slideDots"></div>
</div>

<!-- STATS -->
<div class="stats-row container" id="statsRow"></div>

<!-- CATEGORIES -->
<div class="section" id="categories">
  <div class="section-title">หมวดหมู่แนะนำ</div>
  <div class="cat-grid" id="catGrid"></div>
</div>

<!-- PRODUCTS -->
<div class="section" id="products">
  <div class="section-title">สินค้าแนะนำ</div>
  <div class="prod-grid" id="prodGrid"></div>
</div>

<!-- ADMIN DASHBOARD -->
<div class="admin-panel" id="adminPanel">
  <div class="section-title" style="margin-bottom:14px"><i class="fas fa-cog"></i> ระบบหลังบ้าน (Admin)</div>
  <div class="admin-tabs" id="adminTabs">
    <button class="admin-tab active" data-tab="dash">📊 สถิติ</button>
    <button class="admin-tab" data-tab="settings">⚙️ ตั้งค่าเว็บ</button>
    <button class="admin-tab" data-tab="slides">🖼️ สไลด์</button>
    <button class="admin-tab" data-tab="cats">📁 หมวดหมู่</button>
    <button class="admin-tab" data-tab="prods">📦 สินค้า</button>
    <button class="admin-tab" data-tab="stock">🔑 สต็อกโค้ด</button>
    <button class="admin-tab" data-tab="users">👥 สมาชิก</button>
    <button class="admin-tab" data-tab="topups">💳 ประวัติเติมเงิน</button>
  </div>
  <div class="admin-section active" id="tab-dash"><div class="admin-grid" id="adminStats"></div><h4>สินค้าใกล้หมดสต็อก</h4><div id="lowStock"></div></div>
  <div class="admin-section" id="tab-settings"><h4>ตั้งค่าเว็บไซต์</h4><div id="settingsForm"></div></div>
  <div class="admin-section" id="tab-slides"><h4>จัดการรูปสไลด์</h4><div id="slidesAdmin"></div></div>
  <div class="admin-section" id="tab-cats"><h4>จัดการหมวดหมู่</h4><div id="catsAdmin"></div></div>
  <div class="admin-section" id="tab-prods"><h4>จัดการสินค้า</h4><div id="prodsAdmin"></div></div>
  <div class="admin-section" id="tab-stock"><h4>จัดการสต็อกโค้ด</h4><div id="stockAdmin"></div></div>
  <div class="admin-section" id="tab-users"><h4>จัดการสมาชิก</h4><div id="usersAdmin"></div></div>
  <div class="admin-section" id="tab-topups"><h4>ประวัติการเติมเงิน</h4><div id="topupsAdmin"></div></div>
</div>

<!-- FLOATING CONTACT -->
<div class="float-contact">
  <div class="float-qr" id="lineQrBox">
    <img id="lineQrImg" src="" alt="Line QR" onerror="this.style.display='none'">
    <p>สแกนเพื่อเพิ่มเพื่อน Line<br><span id="lineIdText"></span></p>
  </div>
  <button class="float-btn line" onclick="toggleLineQr()" title="Line"><i class="fab fa-line"></i></button>
  <button class="float-btn fb" onclick="window.open(document.getElementById('fbUrl').value,'_blank')" title="Facebook"><i class="fab fa-facebook-f"></i></button>
</div>
<input type="hidden" id="fbUrl" value="<?= htmlspecialchars($settings['facebook_url']) ?>">

<!-- MODAL AUTH -->
<div class="modal-overlay" id="authModal">
  <div class="modal-box">
    <div class="tabs">
      <button class="tab active" data-tab="login" onclick="switchAuthTab('login')">เข้าสู่ระบบ</button>
      <button class="tab" data-tab="register" onclick="switchAuthTab('register')">สมัครสมาชิก</button>
    </div>
    <div class="tab-content active" id="tab-login">
      <h3>ยินดีต้อนรับกลับมา</h3>
      <p class="sub">เข้าสู่ระบบเพื่อซื้อสินค้าและเติมเงิน</p>
      <div class="form-group"><label>อีเมล / ชื่อผู้ใช้</label><input id="loginEmail" placeholder="email@example.com"></div>
      <div class="form-group"><label>รหัสผ่าน</label><input id="loginPass" type="password" placeholder="••••••••"></div>
      <button class="btn btn-primary" style="width:100%;justify-content:center" onclick="doLogin()">เข้าสู่ระบบ</button>
    </div>
    <div class="tab-content" id="tab-register">
      <h3>สร้างบัญชีใหม่</h3>
      <p class="sub">สมัครฟรี เพียง 30 วินาที</p>
      <div class="form-group"><label>ชื่อผู้ใช้</label><input id="regName" placeholder="ชื่อของคุณ"></div>
      <div class="form-group"><label>อีเมล</label><input id="regEmail" type="email" placeholder="email@example.com"></div>
      <div class="form-group"><label>รหัสผ่าน (อย่างน้อย 6 ตัว)</label><input id="regPass" type="password" placeholder="••••••••"></div>
      <div class="form-group"><label>ยืนยันรหัสผ่าน</label><input id="regPass2" type="password" placeholder="••••••••"></div>
      <button class="btn btn-primary" style="width:100%;justify-content:center" onclick="doRegister()">สมัครสมาชิก</button>
    </div>
    <p style="text-align:center;margin-top:14px"><a onclick="closeModal('authModal')" style="cursor:pointer;color:var(--m);font-size:13px">ปิด</a></p>
  </div>
</div>

<!-- MODAL TOPUP -->
<div class="modal-overlay" id="topupModal">
  <div class="modal-box" style="max-width:440px">
    <h3><i class="fas fa-wallet"></i> เติมเงินเข้าระบบ</h3>
    <p class="sub">เลือกวิธีเติมเงินที่สะดวก</p>
    <div class="tabs">
      <button class="tab active" data-tab="voucher" onclick="switchTopupTab('voucher')">🧧 ซองอั่งเปา TrueMoney</button>
      <button class="tab" data-tab="qr" onclick="switchTopupTab('qr')">📱 โอนผ่าน QR</button>
    </div>
    <div class="tab-content active" id="topup-voucher">
      <div class="qr-box">
        <div style="font-size:40px">🧧</div>
        <div class="qr-name">แจ้งลิงก์ซองอั่งเปา</div>
        <div class="qr-phone">ระบบจะตรวจและเติมเครดิตอัตโนมัติทันที</div>
      </div>
      <div class="form-group"><label>ลิงก์ซองอั่งเปา TrueMoney</label><input id="voucherUrl" placeholder="https://gift.truemoney.com/campaign/?v=..."></div>
      <button class="btn btn-primary" style="width:100%;justify-content:center" onclick="doTopupVoucher()">ยืนยันเติมเงิน</button>
    </div>
    <div class="tab-content" id="topup-qr">
      <div class="qr-box">
        <img id="qrImg" src="" alt="QR PromptPay" onerror="this.parentElement.innerHTML='<div style=\'font-size:40px\'>📱</div><div class=\'qr-name\'>QR PromptPay</div><div class=\'qr-phone\'>อัปโหลดรูป QR ได้ในระบบหลังบ้าน</div>'">
        <div class="qr-name" id="qrName"></div>
        <div class="qr-phone" id="qrPhone"></div>
      </div>
      <div class="form-group"><label>จำนวนเงิน (บาท)</label><input id="topupAmount" type="number" placeholder="เช่น 100"></div>
      <div class="form-group"><label>รหัสอ้างอิงการโอน (หลัง 4 ตัวเลข)</label><input id="topupRef" placeholder="เช่น 1234"></div>
      <button class="btn btn-primary" style="width:100%;justify-content:center" onclick="doTopupQR()">แจ้งโอน / เติมเครดิต</button>
    </div>
    <p style="text-align:center;margin-top:14px"><a onclick="closeModal('topupModal')" style="cursor:pointer;color:var(--m);font-size:13px">ปิด</a></p>
  </div>
</div>

<!-- MODAL HISTORY -->
<div class="modal-overlay" id="historyModal">
  <div class="modal-box" style="max-width:480px;max-height:80vh;overflow-y:auto">
    <h3><i class="fas fa-history"></i> ประวัติการซื้อ</h3>
    <p class="sub">รายการสินค้าที่คุณเคยซื้อ</p>
    <div id="historyList"></div>
    <p style="text-align:center;margin-top:14px"><a onclick="closeModal('historyModal')" style="cursor:pointer;color:var(--m);font-size:13px">ปิด</a></p>
  </div>
</div>

<footer>
  <b id="footerName"><?= htmlspecialchars($settings['site_name']) ?></b> — เว็บขายไอเทมเกม คุณภาพมาตรฐาน<br>
  ระบบปลอดภัย 100% | จัดส่งโค้ดอัตโนมัติทันทีหลังชำระเงิน
</footer>

<script>
// ============== STATE ==============
let DATA = null;
let currentSlide = 0;
let slideTimer = null;

// ============== API HELPER ==============
async function api(endpoint, payload){
  const res = await fetch('?api='+endpoint, {
    method:'POST', headers:{'Content-Type':'application/json'},
    body: JSON.stringify(payload||{})
  });
  const data = await res.json();
  if(data.error) throw new Error(data.error);
  return data;
}

// ============== BOOTSTRAP ==============
async function bootstrap(){
  try{
    DATA = await api('bootstrap');
    document.title = DATA.settings.site_title;
    document.getElementById('siteName').textContent = DATA.settings.site_name;
    document.getElementById('footerName').textContent = DATA.settings.site_name;
    document.getElementById('fbUrl').value = DATA.settings.facebook_url;
    document.getElementById('lineIdText').textContent = DATA.settings.line_id;
    document.getElementById('lineQrImg').src = DATA.settings.line_qr_image_url;
    document.getElementById('qrImg').src = DATA.settings.qr_image_url;
    document.getElementById('qrName').textContent = DATA.settings.qr_recipient_name;
    document.getElementById('qrPhone').textContent = 'เบอร์รับเงิน: ' + DATA.settings.masked_voucher_phone;
    renderSlides(); renderStats(); renderCats(); renderProds(); renderAuth();
    if(DATA.user && DATA.user.role==='admin'){ document.getElementById('adminPanel').classList.add('show'); loadAdminStats(); }
  }catch(e){ Swal.fire('ข้อผิดพลาด', e.message, 'error'); }
}

// ============== RENDER ==============
function renderSlides(){
  const w = document.getElementById('slidesWrap'), d = document.getElementById('slideDots');
  w.innerHTML=''; d.innerHTML='';
  DATA.slides.forEach((s,i)=>{
    const div = document.createElement('div');
    div.className = 'slide' + (i===0?' active':'');
    div.innerHTML = `<img src="${s.image_url}" alt=""><div class="slide-caption"><h2>${s.title}</h2><p>${s.subtitle||''}</p></div>`;
    w.appendChild(div);
    const dot = document.createElement('span');
    dot.className = i===0?'active':'';
    dot.onclick = ()=>goSlide(i);
    d.appendChild(dot);
  });
  clearInterval(slideTimer);
  slideTimer = setInterval(()=>slideMove(1), 5000);
}
function goSlide(i){
  const slides = document.querySelectorAll('.slide'), dots = document.querySelectorAll('#slideDots span');
  currentSlide = (i+slides.length)%slides.length;
  slides.forEach((s,j)=>s.classList.toggle('active', j===currentSlide));
  dots.forEach((d,j)=>d.classList.toggle('active', j===currentSlide));
}
function slideMove(dir){ goSlide(currentSlide+dir); }

function renderStats(){
  const s = DATA.stats;
  document.getElementById('statsRow').innerHTML = `
    <div class="stat-card"><div class="stat-icon s1"><i class="fas fa-users"></i></div><div class="stat-info"><div class="num">${s.total_members.toLocaleString()}</div><div class="lbl">จำนวนสมาชิก</div></div></div>
    <div class="stat-card"><div class="stat-icon s2"><i class="fas fa-box-open"></i></div><div class="stat-info"><div class="num">${s.total_stock.toLocaleString()}</div><div class="lbl">สินค้าในสต็อก</div></div></div>
    <div class="stat-card"><div class="stat-icon s3"><i class="fas fa-shopping-cart"></i></div><div class="stat-info"><div class="num">${s.total_sold.toLocaleString()}</div><div class="lbl">สินค้าที่จำหน่ายไปแล้ว</div></div></div>`;
}

function renderCats(){
  const cats = DATA.categories.filter(c=>c.show_on_home);
  document.getElementById('catGrid').innerHTML = cats.map(c=>`
    <div class="cat-card" onclick="filterByCat(${c.id})">
      <div class="cat-icon">🎮</div>
      <div class="cat-name">${c.name}</div>
      <div class="cat-desc">${c.description||''}</div>
    </div>`).join('') || '<p style="color:var(--m);grid-column:1/-1;text-align:center;padding:20px">ยังไม่มีหมวดหมู่</p>';
}

let catFilter = null;
function filterByCat(id){ catFilter = id; renderProds(); document.getElementById('products').scrollIntoView({behavior:'smooth'}); }

function renderProds(){
  let prods = DATA.products.filter(p=>p.show_on_home);
  if(catFilter) prods = prods.filter(p=>p.category_id===catFilter);
  const catName = id => (DATA.categories.find(c=>c.id===id)||{}).name||'';
  document.getElementById('prodGrid').innerHTML = prods.map(p=>{
    const inStock = parseInt(p.available_stock)>0;
    return `<div class="prod-card">
      <div class="prod-img">${p.image_url?`<img src="${p.image_url}" alt="">`:'🎮'}
        <span class="prod-badge ${inStock?'':'out'}">${inStock?'มีสต็อก':'หมดสต็อก'}</span>
      </div>
      <div class="prod-body">
        <div class="prod-cat">${catName(p.category_id)}</div>
        <div class="prod-name">${p.name}</div>
        <div class="prod-desc">${p.description||''}</div>
        <div class="prod-stock">คงเหลือ <b>${p.available_stock}</b> ชิ้น | ขายแล้ว ${p.sold_count}</div>
        <div class="prod-foot">
          <div class="prod-price">฿${Number(p.price).toLocaleString()}</div>
          <button class="btn-buy" ${inStock?'':'disabled'} onclick="buyProduct(${p.id}, '${p.name.replace(/'/g,"\\'")}', ${p.price})"><i class="fas fa-bolt"></i> ซื้อเลย</button>
        </div>
      </div>
    </div>`;
  }).join('') || '<p style="color:var(--m);grid-column:1/-1;text-align:center;padding:30px">ไม่มีสินค้าในหมวดนี้</p>';
}

// ============== AUTH UI ==============
function renderAuth(){
  const area = document.getElementById('authArea');
  if(!DATA.user){
    area.innerHTML = `<button class="btn btn-outline" onclick="openAuth('login')">เข้าสู่ระบบ</button><button class="btn btn-primary" onclick="openAuth('register')">สมัครสมาชิก</button>`;
    return;
  }
  const u = DATA.user;
  const initial = u.name.charAt(0).toUpperCase();
  area.innerHTML = `
    <div class="profile-wrap">
      <div class="profile-avatar" onclick="toggleDropdown()">${initial}</div>
      <div class="dropdown" id="userDropdown">
        <div class="dd-head"><div class="dd-name">${u.name}</div><div class="dd-credit">💰 เครดิต: ฿${Number(u.credit).toLocaleString()}</div></div>
        <a onclick="openTopup()"><i class="fas fa-wallet"></i> เติมเงิน</a>
        <a onclick="openHistory()"><i class="fas fa-history"></i> ประวัติการซื้อ</a>
        ${u.role==='admin'?'<div class="divider"></div><a onclick="document.getElementById(\'adminPanel\').scrollIntoView({behavior:\'smooth\'})"><i class="fas fa-cog"></i> ระบบหลังบ้าน</a>':''}
        <div class="divider"></div>
        <button onclick="doLogout()"><i class="fas fa-sign-out-alt" style="color:var(--no)"></i> ออกจากระบบ</button>
      </div>
    </div>`;
}
function toggleDropdown(){ document.getElementById('userDropdown').classList.toggle('show'); }
document.addEventListener('click', e=>{ if(!e.target.closest('.profile-wrap')) document.getElementById('userDropdown')?.classList.remove('show'); });

function openAuth(tab){ switchAuthTab(tab); document.getElementById('authModal').classList.add('show'); }
function switchAuthTab(tab){
  document.querySelectorAll('#authModal .tab').forEach(t=>t.classList.toggle('active', t.dataset.tab===tab));
  document.querySelectorAll('#authModal .tab-content').forEach(c=>c.classList.toggle('active', c.id==='tab-'+tab));
}
function openTopup(){ if(!DATA.user){ openAuth('login'); Swal.fire('กรุณาเข้าสู่ระบบ','เข้าสู่ระบบก่อนเติมเงิน','info'); return; } document.getElementById('topupModal').classList.add('show'); }
function switchTopupTab(tab){
  document.querySelectorAll('#topupModal .tab').forEach(t=>t.classList.toggle('active', t.dataset.tab===tab));
  document.querySelectorAll('#topupModal .tab-content').forEach(c=>c.classList.toggle('active', c.id==='topup-'+tab));
}
function closeModal(id){ document.getElementById(id).classList.remove('show'); }

async function doLogin(){
  try{
    const r = await api('login', {email:document.getElementById('loginEmail').value, password:document.getElementById('loginPass').value});
    DATA.user = r.user; closeModal('authModal'); renderAuth();
    Swal.fire({icon:'success',title:'เข้าสู่ระบบสำเร็จ',text:'ยินดีต้อนรับ '+r.user.name,timer:1500,showConfirmButton:false});
  }catch(e){ Swal.fire('เข้าสู่ระบบไม่สำเร็จ', e.message, 'error'); }
}
async function doRegister(){
  try{
    const r = await api('register', {name:document.getElementById('regName').value, email:document.getElementById('regEmail').value, password:document.getElementById('regPass').value, confirm_password:document.getElementById('regPass2').value});
    DATA.user = r.user; closeModal('authModal'); renderAuth();
    Swal.fire({icon:'success',title:'สมัครสมาชิกสำเร็จ',text:'ยินดีต้อนรับ '+r.user.name,timer:1500,showConfirmButton:false});
  }catch(e){ Swal.fire('สมัครไม่สำเร็จ', e.message, 'error'); }
}
async function doLogout(){
  await api('logout'); DATA.user=null; renderAuth(); document.getElementById('adminPanel').classList.remove('show');
  Swal.fire({icon:'info',title:'ออกจากระบบแล้ว',timer:1200,showConfirmButton:false});
}

// ============== BUY ==============
async function buyProduct(id, name, price){
  if(!DATA.user){ openAuth('login'); Swal.fire('กรุณาเข้าสู่ระบบ','เข้าสู่ระบบก่อนซื้อสินค้า','info'); return; }
  const {value:confirm} = await Swal.fire({
    title:'ยืนยันการซื้อ', html:`สินค้า: <b>${name}</b><br>ราคา: <b>฿${Number(price).toLocaleString()}</b><br>เครดิตคงเหลือจะหักลบทันที`,
    icon:'question', showCancelButton:true, confirmButtonText:'ซื้อเลย', cancelButtonText:'ยกเลิก', confirmButtonColor:'#4f46e5'
  });
  if(!confirm) return;
  try{
    const r = await api('buy', {product_id:id});
    DATA.user.credit = r.credit_remaining; renderAuth();
    await Swal.fire({
      icon:'success', title:'ซื้อสำเร็จ! 🎉',
      html:`<b>${r.product_name}</b><br><br>รหัสสินค้าของคุณ:<br><div style="background:#eef2ff;padding:12px;border-radius:10px;font-family:monospace;font-size:14px;word-break:break-all;color:#4f46e5;font-weight:700">${r.code}</div>`,
      showDenyButton:true, denyButtonText:'คัดลอกโค้ด', confirmButtonText:'เสร็จสิ้น'
    }).then(res=>{ if(res.isDenied){ navigator.clipboard?.writeText(r.code); Swal.fire({icon:'success',title:'คัดลอกแล้ว',timer:1000,showConfirmButton:false}); } });
    bootstrap();
  }catch(e){ Swal.fire('ซื้อไม่สำเร็จ', e.message, 'error'); }
}

// ============== TOPUP ==============
async function doTopupVoucher(){
  const url = document.getElementById('voucherUrl').value.trim();
  if(!url){ Swal.fire('กรุณากรอกลิงก์','กรุณาวางลิงก์ซองอั่งเปา TrueMoney','warning'); return; }
  try{
    const r = await api('topup_voucher', {voucher_url:url});
    DATA.user.credit = Number(DATA.user.credit)+r.amount; renderAuth(); closeModal('topupModal');
    Swal.fire({icon:'success',title:'เติมเงินสำเร็จ',text:'เครดิตถูกเพิ่ม '+r.amount+' บาท',timer:2000,showConfirmButton:false});
    document.getElementById('voucherUrl').value='';
  }catch(e){ Swal.fire('เติมเงินไม่สำเร็จ', e.message, 'error'); }
}
async function doTopupQR(){
  const amount = parseFloat(document.getElementById('topupAmount').value);
  const ref = document.getElementById('topupRef').value.trim();
  if(!amount||amount<=0){ Swal.fire('กรุณาระบุจำนวนเงิน','','warning'); return; }
  if(!ref){ Swal.fire('กรุณาระบุรหัสอ้างอิง','หลัง 4 ตัวเลขจากสลิปการโอน','warning'); return; }
  try{
    const r = await api('topup_qr', {amount, reference_code:ref, slip_sha256:''});
    DATA.user.credit = Number(DATA.user.credit)+amount; renderAuth(); closeModal('topupModal');
    Swal.fire({icon:'success',title:'แจ้งโอนสำเร็จ',text:'เครดิตถูกเพิ่ม '+amount+' บาท',timer:2000,showConfirmButton:false});
    document.getElementById('topupAmount').value=''; document.getElementById('topupRef').value='';
  }catch(e){ Swal.fire('ไม่สำเร็จ', e.message, 'error'); }
}

// ============== HISTORY ==============
async function openHistory(){
  closeModal && document.getElementById('userDropdown')?.classList.remove('show');
  document.getElementById('historyModal').classList.add('show');
  try{
    const r = await api('history');
    document.getElementById('historyList').innerHTML = r.orders.length? r.orders.map(o=>`
      <div class="history-item">
        <div><div class="h-name">${o.product_name}</div><div class="h-date">${o.created_at} | ฿${Number(o.price).toLocaleString()}</div></div>
        <div style="display:flex;align-items:center;gap:4px"><span class="h-code">${o.delivered_code}</span><button class="copy-btn" onclick="navigator.clipboard?.writeText('${o.delivered_code}');Swal.fire({icon:'success',title:'คัดลอกแล้ว',timer:800,showConfirmButton:false})"><i class="far fa-copy"></i></button></div>
      </div>`).join('') : '<p style="text-align:center;color:var(--m);padding:20px">ยังไม่มีประวัติการซื้อ</p>';
  }catch(e){ document.getElementById('historyList').innerHTML = '<p style="color:var(--no)">'+e.message+'</p>'; }
}

// ============== CONTACT ==============
function toggleLineQr(){ document.getElementById('lineQrBox').classList.toggle('show'); }

// ============== ADMIN ==============
document.querySelectorAll('.admin-tab').forEach(t=>t.addEventListener('click', ()=>{
  document.querySelectorAll('.admin-tab').forEach(x=>x.classList.remove('active'));
  document.querySelectorAll('.admin-section').forEach(x=>x.classList.remove('active'));
  t.classList.add('active');
  document.getElementById('tab-'+t.dataset.tab).classList.add('active');
  if(t.dataset.tab==='dash') loadAdminStats();
  if(t.dataset.tab==='settings') loadSettingsForm();
  if(t.dataset.tab==='slides') loadSlidesAdmin();
  if(t.dataset.tab==='cats') loadCatsAdmin();
  if(t.dataset.tab==='prods') loadProdsAdmin();
  if(t.dataset.tab==='stock') loadStockAdmin();
  if(t.dataset.tab==='users') loadUsersAdmin();
  if(t.dataset.tab==='topups') loadTopupsAdmin();
}));

async function loadAdminStats(){
  try{
    const r = await api('admin_stats');
    document.getElementById('adminStats').innerHTML = `
      <div class="admin-stat"><div class="n">${r.total_members}</div><div class="l">สมาชิกทั้งหมด</div></div>
      <div class="admin-stat"><div class="n">${r.total_orders}</div><div class="l">ออเดอร์ทั้งหมด</div></div>
      <div class="admin-stat"><div class="n">฿${r.total_revenue.toLocaleString()}</div><div class="l">รายได้จากการขาย</div></div>
      <div class="admin-stat"><div class="n">฿${r.total_topup.toLocaleString()}</div><div class="l">ยอดเติมเงินทั้งหมด</div></div>
      <div class="admin-stat"><div class="n">${r.total_stock}</div><div class="l">โค้ดคงเหลือ</div></div>`;
    document.getElementById('lowStock').innerHTML = r.low_stock.length? '<table class="admin-table"><tr><th>สินค้า</th><th>คงเหลือ</th></tr>'+r.low_stock.map(p=>`<tr><td>${p.name}</td><td><b style="color:var(--no)">${p.stock}</b></td></tr>`).join('')+'</table>' : '<p style="color:var(--ok)">สต็อกเพียงพอทุกชิ้น</p>';
  }catch(e){}
}
function loadSettingsForm(){
  const s = DATA.settings;
  document.getElementById('settingsForm').innerHTML = `
    <div class="inline-form">
      <div class="form-group"><label>ชื่อเว็บไซต์</label><input id="set_site_name" value="${s.site_name}"></div>
      <div class="form-group"><label>Title แท็บเบราว์เซอร์</label><input id="set_site_title" value="${s.site_title}"></div>
    </div>
    <div class="inline-form">
      <div class="form-group"><label>เบอร์รับเงินซองอั่งเปา</label><input id="set_voucher_phone" value=""></div>
      <div class="form-group"><label>ชื่อผู้รับเงิน QR</label><input id="set_qr_recipient_name" value="${s.qr_recipient_name}"></div>
    </div>
    <div class="inline-form">
      <div class="form-group"><label>ลิงก์รูป QR PromptPay</label><input id="set_qr_image_url" value="${s.qr_image_url}"></div>
      <div class="form-group"><label>ลิงก์รูป QR Line</label><input id="set_line_qr_image_url" value="${s.line_qr_image_url}"></div>
    </div>
    <div class="inline-form">
      <div class="form-group"><label>Line ID</label><input id="set_line_id" value="${s.line_id}"></div>
      <div class="form-group"><label>ลิงก์ Facebook</label><input id="set_facebook_url" value="${s.facebook_url}"></div>
    </div>
    <button class="btn btn-primary" onclick="saveSettings()"><i class="fas fa-save"></i> บันทึกการตั้งค่า</button>`;
}
async function saveSettings(){
  const fields = ['site_name','site_title','voucher_phone','qr_recipient_name','qr_image_url','line_qr_image_url','line_id','facebook_url'];
  const payload = {};
  fields.forEach(f=>{ const el=document.getElementById('set_'+f); if(el) payload[f]=el.value; });
  try{ await api('settings_update', payload); await bootstrap(); Swal.fire({icon:'success',title:'บันทึกแล้ว',timer:1200,showConfirmButton:false}); }
  catch(e){ Swal.fire('ข้อผิดพลาด', e.message, 'error'); }
}
async function loadSlidesAdmin(){
  const r = await api('slides_manage',{action:'list'});
  document.getElementById('slidesAdmin').innerHTML = `
    <div class="inline-form">
      <div class="form-group"><label>หัวข้อ</label><input id="sl_title" placeholder="หัวข้อสไลด์"></div>
      <div class="form-group"><label>รองหัวข้อ</label><input id="sl_subtitle" placeholder="รองหัวข้อ"></div>
      <div class="form-group"><label>ลิงก์รูป (1300x400)</label><input id="sl_image" placeholder="https://..."></div>
      <button class="btn btn-primary" onclick="addSlide()"><i class="fas fa-plus"></i> เพิ่ม</button>
    </div>
    <table class="admin-table"><tr><th>รูป</th><th>หัวข้อ</th><th>จัดการ</th></tr>
    ${r.slides.map(s=>`<tr><td><img src="${s.image_url}" style="width:80px;height:30px;object-fit:cover;border-radius:6px"></td><td>${s.title}</td><td><button class="btn-sm danger" onclick="delSlide(${s.id})"><i class="fas fa-trash"></i> ลบ</button></td></tr>`).join('')}
    </table>`;
}
async function addSlide(){
  await api('slides_manage',{action:'add',title:sl_title.value,subtitle:sl_subtitle.value,image_url:sl_image.value});
  loadSlidesAdmin(); bootstrap(); Swal.fire({icon:'success',title:'เพิ่มแล้ว',timer:1000,showConfirmButton:false});
}
async function delSlide(id){ await api('slides_manage',{action:'delete',id}); loadSlidesAdmin(); bootstrap(); }

async function loadCatsAdmin(){
  const r = await api('categories_manage',{action:'list'});
  document.getElementById('catsAdmin').innerHTML = `
    <div class="inline-form">
      <div class="form-group"><label>ชื่อหมวดหมู่</label><input id="cat_name" placeholder="เช่น ROBLOX"></div>
      <div class="form-group"><label>คำอธิบาย</label><input id="cat_desc" placeholder="คำอธิบายสั้นๆ"></div>
      <div class="form-group"><label>ลิงก์รูป</label><input id="cat_img" placeholder="https://..."></div>
      <button class="btn btn-primary" onclick="addCat()"><i class="fas fa-plus"></i> เพิ่ม</button>
    </div>
    <table class="admin-table"><tr><th>ชื่อ</th><th>แสดงหน้าแรก</th><th>จัดการ</th></tr>
    ${r.categories.map(c=>`<tr><td>${c.name}</td><td>${c.show_on_home?'<b style="color:var(--ok)">เปิด</b>':'<b style="color:var(--no)">ปิด</b>'}</td>
    <td><button class="btn-sm warn" onclick="toggleCat(${c.id},${c.show_on_home?0:1})">${c.show_on_home?'ปิดแสดง':'เปิดแสดง'}</button><button class="btn-sm danger" onclick="delCat(${c.id})">ลบ</button></td></tr>`).join('')}
    </table>`;
}
async function addCat(){ await api('categories_manage',{action:'add',name:cat_name.value,description:cat_desc.value,image_url:cat_img.value}); loadCatsAdmin(); bootstrap(); }
async function toggleCat(id,val){ await api('categories_manage',{action:'update',id,show_on_home:val,name:'',description:'',image_url:''}); loadCatsAdmin(); bootstrap(); }
async function delCat(id){ await api('categories_manage',{action:'delete',id}); loadCatsAdmin(); bootstrap(); }

async function loadProdsAdmin(){
  const r = await api('products_manage',{action:'list'});
  const cats = (await api('categories_manage',{action:'list'})).categories;
  const catOpts = cats.map(c=>`<option value="${c.id}">${c.name}</option>`).join('');
  document.getElementById('prodsAdmin').innerHTML = `
    <div class="inline-form">
      <div class="form-group"><label>หมวดหมู่</label><select id="pd_cat">${catOpts}</select></div>
      <div class="form-group"><label>ชื่อสินค้า</label><input id="pd_name" placeholder="ชื่อสินค้า"></div>
      <div class="form-group"><label>ราคา (บาท)</label><input id="pd_price" type="number" placeholder="0"></div>
      <div class="form-group"><label>ลิงก์รูป</label><input id="pd_img" placeholder="https://..."></div>
      <button class="btn btn-primary" onclick="addProd()"><i class="fas fa-plus"></i> เพิ่ม</button>
    </div>
    <table class="admin-table"><tr><th>สินค้า</th><th>หมวด</th><th>ราคา</th><th>สต็อก</th><th>เปิดขาย</th><th>จัดการ</th></tr>
    ${r.products.map(p=>`<tr><td>${p.name}</td><td>${p.category_name||'-'}</td><td>฿${Number(p.price).toLocaleString()}</td><td><b>${p.stock}</b></td>
    <td>${p.is_active?'<b style="color:var(--ok)">เปิด</b>':'<b style="color:var(--no)">ปิด</b>'}</td>
    <td><button class="btn-sm warn" onclick="toggleProd(${p.id},${p.is_active?0:1})">${p.is_active?'ปิด':'เปิด'}</button><button class="btn-sm info" onclick="manageStock(${p.id},'${p.name.replace(/'/g,"\\'")}')">โค้ด</button><button class="btn-sm danger" onclick="delProd(${p.id})">ลบ</button></td></tr>`).join('')}
    </table>`;
}
async function addProd(){ await api('products_manage',{action:'add',category_id:pd_cat.value,name:pd_name.value,description:'',price:pd_price.value,image_url:pd_img.value}); loadProdsAdmin(); bootstrap(); }
async function toggleProd(id,val){ await api('products_manage',{action:'update',id,is_active:val,category_id:0,name:'',description:'',price:0,image_url:'',show_on_home:1}); loadProdsAdmin(); bootstrap(); }
async function delProd(id){ await api('products_manage',{action:'delete',id}); loadProdsAdmin(); bootstrap(); }

async function manageStock(pid, pname){
  const {value:codes} = await Swal.fire({
    title:'เพิ่มโค้ดสต็อก: '+pname, input:'textarea', inputPlaceholder:'วางโค้ดทีละบรรทัด\nCODE-001\nCODE-002',
    confirmButtonText:'เพิ่มสต็อก', showCancelButton:true, inputValidator:v=>!v?.trim()?'กรุณาวางโค้ดอย่างน้อย 1 บรรทัด':null
  });
  if(!codes) return;
  const r = await api('stock_manage',{action:'add',product_id:pid,codes});
  Swal.fire({icon:'success',title:'เพิ่มแล้ว '+r.added+' รายการ',timer:1500,showConfirmButton:false});
  loadProdsAdmin(); bootstrap();
}
function loadStockAdmin(){ document.getElementById('stockAdmin').innerHTML = '<p style="color:var(--m)">ไปที่แท็บ <b>สินค้า</b> แล้วกดปุ่ม <b>โค้ด</b> ที่สินค้าที่ต้องการเพื่อจัดการสต็อก</p>'; }

async function loadUsersAdmin(){
  const r = await api('users_manage',{action:'list'});
  document.getElementById('usersAdmin').innerHTML = `<table class="admin-table"><tr><th>ชื่อ</th><th>อีเมล</th><th>ยศ</th><th>เครดิต</th><th>จัดการ</th></tr>
    ${r.users.map(u=>`<tr><td>${u.name}</td><td>${u.email}</td><td><b style="color:${u.role==='admin'?'var(--gold)':'var(--m)'}">${u.role==='admin'?'👑 Admin':'👤 User'}</b></td><td>฿${Number(u.credit).toLocaleString()}</td>
    <td>
      <button class="btn-sm info" onclick="addCredit(${u.id})">+/- เครดิต</button>
      <button class="btn-sm warn" onclick="resetPass(${u.id})">รีเซ็ตรหัส</button>
      <button class="btn-sm warn" onclick="toggleRole(${u.id},'${u.role}')">${u.role==='admin'?'降为User':'เป็นAdmin'}</button>
      ${u.role!=='admin'?`<button class="btn-sm danger" onclick="delUser(${u.id})">ลบ</button>`:''}
    </td></tr>`).join('')}
    </table>`;
}
async function addCredit(id){ const {value:v}=await Swal.fire({title:'เพิ่ม/ลด เครดิต',input:'number',inputLabel:'จำนวน (บวก=เพิ่ม ลบ=ลด)',inputValue:'0'}); if(v==null) return; await api('users_manage',{action:'update',id,credit_delta:parseFloat(v)}); loadUsersAdmin(); }
async function resetPass(id){ const {value:v}=await Swal.fire({title:'รีเซ็ตรหัสผ่าน',input:'text',inputPlaceholder:'รหัสผ่านใหม่'}); if(!v) return; await api('users_manage',{action:'update',id,new_password:v}); Swal.fire({icon:'success',title:'รีเซ็ตแล้ว',timer:1200,showConfirmButton:false}); }
async function toggleRole(id, cur){ await api('users_manage',{action:'update',id,role:cur==='admin'?'user':'admin'}); loadUsersAdmin(); }
async function delUser(id){ const {value:c}=await Swal.fire({title:'ลบสมาชิก?',icon:'warning',showCancelButton:true}); if(!c) return; await api('users_manage',{action:'delete',id}); loadUsersAdmin(); }

async function loadTopupsAdmin(){
  const r = await api('topup_logs');
  document.getElementById('topupsAdmin').innerHTML = `<table class="admin-table"><tr><th>ผู้ใช้</th><th>วิธี</th><th>จำนวน</th><th>อ้างอิง</th><th>สถานะ</th><th>เวลา</th></tr>
    ${r.logs.map(l=>`<tr><td>${l.user_name||'-'}</td><td>${l.method==='voucher'?'🧧 อั่งเปา':'📱 QR'}</td><td><b style="color:var(--ok)">฿${Number(l.amount).toLocaleString()}</b></td><td style="font-family:monospace;font-size:11px">${l.reference_code}</td><td><b style="color:var(--ok)">อนุมัติ</b></td><td style="font-size:11px">${l.created_at}</td></tr>`).join('')}
    </table>`;
}

// เริ่มต้น
bootstrap();
</script>
</body>
</html>
