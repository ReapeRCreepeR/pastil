<?php
header('Content-Type: application/json');
ini_set('display_errors', 1);
error_reporting(E_ALL);

$conn = new mysqli("localhost", "root", "", "pastil");

if ($conn->connect_error) {
    http_response_code(500);
    echo json_encode(['status'=>'error', 'message'=>'DB connection failed: ' . $conn->connect_error]);
    exit;
}

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $raw = file_get_contents("php://input");
    $data = json_decode($raw, true);

    // DEBUG: Uncomment to see incoming data
    // file_put_contents('debug_order.log', print_r($data, true));

    if (!isset($data['order']) || !is_array($data['order']) || count($data['order']) === 0) {
        echo json_encode(['status'=>'error', 'message'=>'No order items received']);
        exit;
    }
    if (!isset($data['customer_name']) || trim($data['customer_name']) === '') {
        echo json_encode(['status'=>'error', 'message'=>'No customer name received']);
        exit;
    }

    $order = $data['order'];
    $customer_name = $conn->real_escape_string(trim($data['customer_name']));
    $total = 0;
    $validated_items = [];

    foreach ($order as $i => $item) {
        // Validate each item strictly
        if (
            !is_array($item) ||
            !isset($item['name'], $item['qty'], $item['price']) ||
            !is_string($item['name']) ||
            $item['name'] === "" ||
            !is_numeric($item['qty']) ||
            intval($item['qty']) <= 0 ||
            !is_numeric($item['price'])
        ) {
            echo json_encode([
                'status'=>'error',
                'message'=>"Invalid item data at position $i: " . json_encode($item)
            ]);
            exit;
        }
        // Sanitize and build valid item array for db
        $item_name = $conn->real_escape_string($item['name']);
        $item_qty = intval($item['qty']);
        $item_price = floatval($item['price']);
        $validated_items[] = [
            'name' => $item_name,
            'qty' => $item_qty,
            'price' => $item_price
        ];
        $total += $item_qty * $item_price;
    }
    $items_json = $conn->real_escape_string(json_encode($validated_items));
    $now = date("Y-m-d H:i:s");

    $sql = "INSERT INTO orders (customer_name, order_time, items, total) VALUES ('$customer_name', '$now', '$items_json', $total)";
    if ($conn->query($sql)) {
        $order_id = $conn->insert_id;
        echo json_encode(['status'=>'success', 'order_id'=>$order_id]);
    } else {
        http_response_code(500);
        echo json_encode(['status'=>'error', 'message'=>$conn->error]);
    }
    exit;
}

// GET: customer_name filter support
if ($_SERVER['REQUEST_METHOD'] === 'GET') {
    $customer_name = isset($_GET['customer_name']) ? $conn->real_escape_string($_GET['customer_name']) : '';
    if ($customer_name !== '') {
        $rs = $conn->query("SELECT * FROM orders WHERE customer_name='$customer_name' ORDER BY id DESC");
    } else {
        $rs = $conn->query("SELECT * FROM orders ORDER BY id DESC");
    }
    $orders = [];
    while ($row = $rs->fetch_assoc()) {
        $row['items'] = json_decode($row['items'], true);
        $orders[] = $row;
    }
    echo json_encode(['status'=>'success', 'orders'=>$orders]);
}
?>
