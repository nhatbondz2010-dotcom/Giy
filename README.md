conn = sqlite3.connect('vouchers.db')
cursor = conn.cursor()

# Kiểm tra tồn kho
cursor.execute("SELECT code FROM vouchers WHERE id = ? AND is_sold = 0", (voucher_id,))
row = cursor.fetchone()

if not row:
    conn.close()
    return jsonify({"status": "error", "message": "Voucher không tồn tại hoặc đã được mua"}), 400

# Đánh dấu đã bán
cursor.execute("UPDATE vouchers SET is_sold = 1 WHERE id = ?", (voucher_id,))
conn.commit()
conn.close()

return jsonify({
    "status": "success", 
    "message": "Thanh toán thành công!", 
    "voucher_code": row[0]
})
