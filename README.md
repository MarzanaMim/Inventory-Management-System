# Inventory-Management-System


#include <stdio.h>
#include <stdlib.h>
#include <string.h>
// Define structure for inventory items
typedef struct {
int id;
char name[50];
float price;
int quantity;
} Item;
// Function declarations
void addItem();
void updateItem();
void deleteItem();
void viewInventory();
void searchItem();
void generateReport();
void authenticateUser();
void purchaseItem();
void sellItem();
FILE *file;
int main() {
int choice;
authenticateUser(); // User login
do {
printf("¥n===== Inventory Management System =====¥n");
printf("1. Add New Item¥n2. Update Item¥n3. Delete Item¥n4. View Inventory¥n5. Search Item¥n6. Generate Report¥n7. Purchase Item¥n8. Sell Item¥n9. Exit¥n");
printf("Enter your choice: ");
scanf("%d", &choice);
switch(choice) {
case 1: addItem(); break;
case 2: updateItem(); break;
case 3: deleteItem(); break;
case 4: viewInventory(); break;
case 5: searchItem(); break;
case 6: generateReport(); break;
case 7: purchaseItem(); break;
case 8: sellItem(); break;
case 9: printf("Exiting system...¥n"); exit(0);
default: printf("Invalid choice! Try again.¥n");
}
} while(choice != 9);
return 0;
}
// User authentication
void authenticateUser() {
char username[20], password[20];
printf("Enter Username: ");
scanf("%s", username);
printf("Enter Password: ");
scanf("%s", password);
if(strcmp(username, "Marzana") == 0 && strcmp(password, "1234") == 0) {
printf("Login Successful!¥n");
} else {
printf("Invalid credentials! Exiting...¥n");
exit(0);
}
}
// Function to add a new item
void addItem() {
Item item;
file = fopen("inventory.dat", "ab");
printf("Enter item ID: ");
scanf("%d", &item.id);
printf("Enter item name: ");
scanf("%s", item.name);
printf("Enter price: ");
scanf("%f", &item.price);
printf("Enter quantity: ");
scanf("%d", &item.quantity);
fwrite(&item, sizeof(Item), 1, file);
fclose(file);
printf("Item added successfully!¥n");
}
// Function to update an item
void updateItem() {
int id, found = 0;
Item item;
FILE *temp;
file = fopen("inventory.dat", "rb");
temp = fopen("temp.dat", "wb");
printf("Enter item ID to update: ");
scanf("%d", &id);
while(fread(&item, sizeof(Item), 1, file)) {
if(item.id == id) {
found = 1;
printf("Enter new name: ");
scanf("%s", item.name);
printf("Enter new price: ");
scanf("%f", &item.price);
printf("Enter new quantity: ");
scanf("%d", &item.quantity);
}
fwrite(&item, sizeof(Item), 1, temp);
}
fclose(file);
fclose(temp);
remove("inventory.dat");
rename("temp.dat", "inventory.dat");
if(found)
printf("Item updated successfully!¥n");
else
printf("Item not found!¥n");
}
// Function to delete an item
void deleteItem() {
int id, found = 0;
Item item;
FILE *temp;
file = fopen("inventory.dat", "rb");
temp = fopen("temp.dat", "wb");
printf("Enter item ID to delete: ");
scanf("%d", &id);
while(fread(&item, sizeof(Item), 1, file)) {
if(item.id == id) {
found = 1;
} else {
fwrite(&item, sizeof(Item), 1, temp);
}
}
fclose(file);
fclose(temp);
remove("inventory.dat");
rename("temp.dat", "inventory.dat");
if(found)
printf("Item deleted successfully!¥n");
else
printf("Item not found!¥n");
}
// Function to view inventory
void viewInventory() {
Item item;
file = fopen("inventory.dat", "rb");
printf("¥nID¥tName¥tPrice¥tQuantity¥n");
while(fread(&item, sizeof(Item), 1, file)) {
printf("%d¥t%s¥t%.2f¥t%d¥n", item.id, item.name, item.price, item.quantity);
}
fclose(file);
}
// Function to search an item
void searchItem() {
int id, found = 0;
Item item;
file = fopen("inventory.dat", "rb");
printf("Enter item ID to search: ");
scanf("%d", &id);
while(fread(&item, sizeof(Item), 1, file)) {
if(item.id == id) {
printf("¥nItem Found!¥n");
printf("ID: %d¥nName: %s¥nPrice: %.2f¥nQuantity: %d¥n", item.id, item.name, item.price, item.quantity);
found = 1;
break;
}
}
fclose(file);
if(!found)
printf("Item not found!¥n");
}
// Function to generate report
void generateReport() {
Item item;
float totalValue = 0;
file = fopen("inventory.dat", "rb");
printf("¥n===== Inventory Report =====¥n");
printf("ID¥tName¥tPrice¥tQuantity¥tTotal Value¥n");
while(fread(&item, sizeof(Item), 1, file)) {
float itemValue = item.price * item.quantity;
printf("%d¥t%s¥t%.2f¥t%d¥t¥t%.2f¥n", item.id, item.name, item.price, item.quantity, itemValue);
totalValue += itemValue;
}
printf("¥nTotal Inventory Value: $%.2f¥n", totalValue);
fclose(file);
}
// Function to purchase (increase quantity)
void purchaseItem() {
int id, qty, found = 0;
Item item;
FILE *temp;
file = fopen("inventory.dat", "rb");
temp = fopen("temp.dat", "wb");
printf("Enter item ID to purchase: ");
scanf("%d", &id);
printf("Enter quantity to add: ");
scanf("%d", &qty);
while(fread(&item, sizeof(Item), 1, file)) {
if(item.id == id) {
item.quantity += qty;
found = 1;
}
fwrite(&item, sizeof(Item), 1, temp);
}
fclose(file);
fclose(temp);
remove("inventory.dat");
rename("temp.dat", "inventory.dat");
if(found)
printf("Stock updated successfully!¥n");
else
printf("Item not found!¥n");
}
// Function to sell (decrease quantity)
void sellItem() {
int id, qty, found = 0;
Item item;
FILE *temp;
file = fopen("inventory.dat", "rb");
temp = fopen("temp.dat", "wb");
printf("Enter item ID to sell: ");
scanf("%d", &id);
printf("Enter quantity to sell: ");
scanf("%d", &qty);
while(fread(&item, sizeof(Item), 1, file)) {
if(item.id == id) {
if(item.quantity >= qty) {
item.quantity -= qty;
printf("Sale successful!¥n");
} else {
printf("Not enough stock to sell!¥n");
}
found = 1;
}
fwrite(&item, sizeof(Item), 1, temp);
}
fclose(file);
fclose(temp);
remove("inventory.dat");
rename("temp.dat", "inventory.dat");
if(!found)
printf("Item not found!¥n");
}
