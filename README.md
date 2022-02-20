# Intellegend-Final-Project - Ecommerce Customer Churn Prediction
## Stage 1
EDA Data

Summary dari EDA:
- split training and unseen data (test data)
- Drop duplicated data
- Drop customerID
- PreferredLoginDevice: merubah category "Mobile Phone" menjadi "Phone"
- PreferredPaymentMode: merubah category CC menjadi Credit Card dan COD menjadi Cash on Delivery
- Handle missing value
- Feature selection: Tenure, CityTier, WarehouseToHome, Complain, DaySinceLastOrder, CashbackAmount
- Prevent multicolinearity
- boxcox transform: Tenure, WarehouseToHome, DaySinceLastOrder
- data sudah dilihat pada EDA bahwa tidak ada outlier ( data masuk akal dan terlihat nyata )

## Stage 2
Melakukan semua hal yang telah di list pada summary stage 1

## Stage 3
**Split Data Train & Test**: Split data train dan test sudah dilakukan pada tahap preprocessing untuk menghindari terjadinya data leak, karena data test adalah unseen data.
Modeling: Model yang digunakan adalah 
Logistic Regression (Baseline)
Decision Tree
KNN
SVM
**Random Forest** (best model) Model ini untuk sementara dipilih karena memiliki score precision dan ROC_AUC yang paling tinggi disbanding model lainnya.
Gradient Boosting
XGBoost
AdaBoost
CatBoost

**Model Evaluation**: 
Pada kasus Ecommerce churn prediction ini, kami menggunakan Recall sebagai metrics utama karena tujuan utama dari prediksi model ini adalah mencegah customer untuk churn jika ia terdeteksi churn, maka dari itu dengan metrics recall prediksi kami dapat berfokus sebanyak-banyaknya customer yang berpotensi untuk churn untuk mencegah mereka untuk churn.

Customer yang diprediksi akan churn akan diberikan kupon atau penawaran spesial agar mereka tidak churn, namun karena dataset ini mempunyai target yang imbalance, untuk mencegah memberikan terlalu banyak kupon kepada customer yang tidak berpotensi churn maka kami akan memakai metrics ROC_AUC sebagai tambahan.

Walaupun model yangn sudah di train sudah melewati baseline model yang berarti model sudah cukup baik, namun Model belum best-fit karena masih dapat ditemukan model yang lebih baik lagi dengan mencoba menggunakan feature-feature lain pada data yang terkena drop pada tahap preprocess (stage 2), serta dapat mentuning hyperparameter dengan lebih baik lagi

**Hyperparameter Tuning**: Terdapat beberapa hyperparameter yang digunakan untuk mentuning masing-masing model, namunn hyperparameter yang digunakan untuk mentuning best model adalah:
- n_estimators : untuk mengatur jumlah tree dalam randomForest model
- min_samples_split : minimum sample yang diperlukan untuk melakukan split pada suatu node
- min_samples_leaf: minimum sample yang diperlukan dalam suatu leaf, jika kurang dari min_samples_leaf maka leaf akan di prune
- max_features: maximum feature yang digunakan untuk splitting node pada tree
- max_depth: maximum kedalaman yang dapat dibentuk oleh tree
- bootstrap: jika true maka tiap tree akan dibuat dengan sample data, jika false semua data akan digunakan untuk semua tree

Eksperimen yang telah dilakukan adalah mencoba train semua model yang belum di tuning hyperparameternya lalu membandingkan score mereka, lalu mencoba train semua model sambal mentuning hyperparameternya lalu membandingkan score mereka. Selain itu mencoba feature scaling dari feature importances yang telah didapat. Dari semua percobaan, untuk saat ini model yang terbaik adalah model RandomForest

**Feature yang mempunyai pengaruh besar adalah**:
- CashbackAmount
- Tenure
- WarehouseToHome
- DaySinceLastOrder
- Complain

Melihat dari feature-feature yang penting diatas dapat ditarik kesimpulan bahwa, untuk mencegah terjadinya customer churn, pelanggan butuh diberikan Cashback yang cukup besar sampai setidaknya 10 bulan (10 bulan disini merupakan median dimana customer diprediksi tidak churn, customer diprediksi churn jika tenure dibawah 1 bulan). Hal ini dapat kita tarik sebagai kesimpulan karena terlihat CashbackAmount dan Tenure merupakan feature yang paling berpengaruh pada churn customer. 

Selain itu jumlah warehouse juga perlu diperbanyak di tempat yang memiliki banyak customer, karena warehouse yang terlalu jauh menyebabkan customer churn. Dan yang terakhir adalah harus menjaga DaySinceLastOrder yang stabil, jika customer terlalu sering atau terlalu jarang menggunakan Ecommerce ini, maka customer tersebut akan berpotensi churn karena itulah customer perlu diberikan voucher mingguan yang membuat belanja mereka teratur setiap minggunya. Dan yang terakhir adalah Complain, jika customer memiliki complain harus diselesaikan dengan segera, karena complain yang tidak diselesaikan dapat menyebabkan customer churn

