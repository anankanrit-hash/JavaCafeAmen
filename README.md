# JavaCafeAmen
import java.util.Scanner;
public class Main
{
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String[] name = {"Americano", "Cappuchino", "BlackCoffee", "Tea"};
        double[] price = {67.00 , 55.00 , 20.00 , 76.00};

        System.out.println("             --- Welcome to Cafe game&show&gutgusgus&ice ---");
        System.out.println("                          -----เมนูสินค้า-----");
        
        for (int x = 0; x < name.length; x++) {
            System.out.println((x + 1) + "." + name[x] + " " + price[x] + " บาท");
        }
        System.out.println("0.จบการสั่งซื้อ");
        System.out.println("-------------------");

        String[] orderName = new String[20];
        double[] orderTotal = new double[20];
        int[] orderQty = new int[20];
        double[] orderPrice = new double[20];
        int count = 0;

        double grandTotal = 0;
        int totalQty = 0;
        boolean orderMore = true;

        while (orderMore) {
            System.out.print("เลือกรายการ (0-6): ");
            int choice = sc.nextInt();

            if (choice == 0) {
                System.out.println("จบการสั่งซื้อ");
                break;
            }
            if (choice < 1 || choice > 6) {
                System.out.println("เลือกผิด กรุณาเลือกใหม่");
                continue;
            }

            System.out.print("จำนวน (แก้ว): ");
            int qty = sc.nextInt();
            if (qty <= 0) {
                qty = 1;
            }

            double price2 = calculatePrice(price[choice - 1], qty);

            System.out.println(">> เพิ่ม " + name[choice - 1] + " x " + qty + " แก้ว");

            orderName[count] = name[choice - 1];
            orderQty[count] = qty;
            orderPrice[count] = price[choice - 1];
            orderTotal[count] = price2;
            count++;

            grandTotal += price2;
            totalQty += qty;
            
            boolean validAns = false;
            while (!validAns) {
                System.out.print("ต้องการสั่งเพิ่มหรือไม่? (y/n): ");
                String ans = sc.next();

                if (ans.equalsIgnoreCase("y")) {
                    validAns = true;
                }
                else if (ans.equalsIgnoreCase("n")) {
                    orderMore = false;
                    validAns = true;
                }
                else {
                    System.out.println("กรุณาตอบ y หรือ n เท่านั้น");
                }
            }
        }

        double discount = calculateDiscount(grandTotal, totalQty);
        double netTotal = grandTotal - discount;

        printReceipt(orderName, orderQty, orderPrice, orderTotal, count, grandTotal, discount, netTotal);
    }

    static double calculatePrice(double price, int qty) {
        return price * qty;
    }

    static double calculateDiscount(double total, int totalQty) {
        if (totalQty >= 5) {
            return total * 0.10;
        }
        return 0;
    }

    static void printReceipt(String[] orderName, int[] orderQty, double[] orderPrice, double[] orderTotal,
                              int count, double grandTotal, double discount, double netTotal) {
        System.out.println("\n========== ใบเสร็จรับเงิน ==========");
        System.out.println("รายการ\tจำนวน\tราคา/หน่วย\tรวม");
        System.out.println("-------------------------------------");
        for (int i = 0; i < count; i++) {
            System.out.println(orderName[i] + "\t" + orderQty[i] + "\t" + orderPrice[i] + "\t" + orderTotal[i]);
        }
        System.out.println("-------------------------------------");
        System.out.println("รวมก่อนส่วนลด: " + grandTotal + " บาท");
        System.out.println("ส่วนลด: " + discount + " บาท");
        System.out.println("ยอดสุทธิ: " + netTotal + " บาท");
        System.out.println("=====================================");
        System.out.println("        ขอบคุณที่ใช้บริการ");
    }
}
