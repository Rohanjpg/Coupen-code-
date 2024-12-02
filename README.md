# Coupen-code-
this coupen code system for wordpress website . If you want use it then cope code and past in function.php file in your admin panel , this coupen code automatic  generate after customer buy some product in your website and this coupen code automatic send to customer email id the customer use coupen code for next order only 1 time .
only 5% less amount in coupen function ,  if you want custmiztion you also edit.

gust copy and past.

// coupen code use 

add_action('woocommerce_order_status_completed', 'send_thank_you_email_with_coupon_on_order_complete', 10, 1);

function send_thank_you_email_with_coupon_on_order_complete($order_id) {
    // Get the order object
    $order = wc_get_order($order_id);

    // Get customer email
    $customer_email = $order->get_billing_email();

    // Generate a coupon code (e.g., "5% OFF")
    $coupon_code = generate_coupon_code($order_id);

    // Email subject and message
    $subject = 'Thank you for your order!';

    // Build the HTML message
    $message = '<html><body>';
    $message .= '<h2>Thank you for your order, ' . $order->get_billing_first_name() . '!</h2>';
    $message .= '<p>Your order has been completed successfully.</p>';
    $message .= '<p>As a token of our appreciation, here is a coupon code for 5% off your next order:</p>';
    // Add red color to the coupon code
    $message .= '<p><strong><span style="color: red;">Coupon Code: ' . $coupon_code . '</span></strong></p>';
    $message .= '<p>The coupon is valid for 60 days. Don\'t miss out!</p>';
    $message .= '<p>Best regards,<br>Kriti life</p>';
    $message .= '</body></html>';

    // Set email headers to send HTML email
    $headers = array('Content-Type: text/html; charset=UTF-8');

    // Send the email to the customer
    wp_mail($customer_email, $subject, $message, $headers);
}

function generate_coupon_code($order_id) {
    // Create a unique coupon code based on the order ID (e.g., 'ORDER5OFF12345')
    $coupon_code = 'ORDER5OFF' . $order_id;

    // Check if the coupon code already exists, and create a new one if it does
    if (coupon_exists($coupon_code)) {
        $coupon_code = 'ORDER5OFF' . rand(1000, 9999); // Generate a new coupon code if the original one exists
    }

    // Create the coupon
    $coupon = new WC_Coupon();
    $coupon->set_code($coupon_code);
    $coupon->set_discount_type('percent');  // Set the discount type as percentage
    $coupon->set_amount(5);  // 5% discount
    $coupon->set_individual_use(true); // Ensure it can only be used alone
    $coupon->set_usage_limit(1);  // Limit it to one use per coupon
    $coupon->set_usage_limit_per_user(1); // Limit usage per user to 1

    // Set the expiration date to 60 days from now
    $expiration_date = strtotime('+60 days');
    $coupon->set_date_expires($expiration_date);

    // Save the coupon
    $coupon->save();

    return $coupon_code;
}

function coupon_exists($coupon_code) {
    $coupon = new WC_Coupon($coupon_code);
    return $coupon->get_id() ? true : false;
}

