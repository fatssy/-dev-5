import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Kişilik Anketi',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        visualDensity: VisualDensity.adaptivePlatformDensity,
      ),
      home: PersonalitySurveyPage(),
    );
  }
}

class PersonalitySurveyPage extends StatefulWidget {
  @override
  _PersonalitySurveyPageState createState() => _PersonalitySurveyPageState();
}

class _PersonalitySurveyPageState extends State<PersonalitySurveyPage> {
  // Form kontrolcüleri
  final TextEditingController _nameController = TextEditingController();
  final TextEditingController _surnameController = TextEditingController();
  
  // Dropdown değeri
  String? _selectedGender;
  
  // Checkbox değerleri
  bool _music = false;
  bool _sports = false;
  bool _reading = false;
  bool _travel = false;
  bool _cooking = false;
  
  // Switch değeri
  bool _smokingStatus = false;
  
  // Slider değeri
  double _cigarettesPerDay = 0;
  
  final List<String> _genders = ['Erkek', 'Kadın', 'Diğer', 'Belirtmek İstemiyorum'];

  @override
  void dispose() {
    _nameController.dispose();
    _surnameController.dispose();
    super.dispose();
  }

  void _submitForm() {
    // Form verilerini topla
    String name = _nameController.text;
    String surname = _surnameController.text;
    
    List<String> hobbies = [];
    if (_music) hobbies.add('Müzik');
    if (_sports) hobbies.add('Spor');
    if (_reading) hobbies.add('Okuma');
    if (_travel) hobbies.add('Seyahat');
    if (_cooking) hobbies.add('Yemek Pişirme');
    
    // Bilgileri göster
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: Text('Form Bilgileri'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text('Ad: $name'),
            Text('Soyad: $surname'),
            Text('Cinsiyet: ${_selectedGender ?? "Seçilmemiş"}'),
            Text('Hobiler: ${hobbies.join(", ")}'),
            Text('Sigara: ${_smokingStatus ? "Evet" : "Hayır"}'),
            if (_smokingStatus) Text('Günlük Sigara: ${_cigarettesPerDay.round()} adet'),
          ],
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: Text('Tamam'),
          ),
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Kişilik Anketi'),
        backgroundColor: Colors.blue[700],
        foregroundColor: Colors.white,
        elevation: 2,
      ),
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
            colors: [Colors.blue[50]!, Colors.white],
          ),
        ),
        child: SingleChildScrollView(
          padding: EdgeInsets.all(20),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: [
              // Başlık bölümü
              Container(
                padding: EdgeInsets.all(16),
                decoration: BoxDecoration(
                  color: Colors.white,
                  borderRadius: BorderRadius.circular(12),
                  boxShadow: [
                    BoxShadow(
                      color: Colors.grey.withOpacity(0.2),
                      blurRadius: 8,
                      offset: Offset(0, 4),
                    ),
                  ],
                ),
                child: Column(
                  children: [
                    Icon(
                      Icons.person_outline,
                      size: 48,
                      color: Colors.blue[700],
                    ),
                    SizedBox(height: 8),
                    Text(
                      'Bu hafta sizden ödev olarak sizden derste gördüğümüz UI ve Layout elemanlarıyla bir Kişilik anketi sayfası yapmanız bekleniyor.',
                      style: TextStyle(
                        fontSize: 16,
                        fontWeight: FontWeight.w500,
                        color: Colors.grey[700],
                      ),
                      textAlign: TextAlign.center,
                    ),
                    SizedBox(height: 12),
                    Text(
                      'Bu sayfada zorunlu olarak olması gereken elemanlar:',
                      style: TextStyle(
                        fontSize: 14,
                        fontWeight: FontWeight.bold,
                        color: Colors.blue[800],
                      ),
                    ),
                  ],
                ),
              ),
              
              SizedBox(height: 20),
              
              // Form bölümü
              Container(
                padding: EdgeInsets.all(20),
                decoration: BoxDecoration(
                  color: Colors.white,
                  borderRadius: BorderRadius.circular(12),
                  boxShadow: [
                    BoxShadow(
                      color: Colors.grey.withOpacity(0.2),
                      blurRadius: 8,
                      offset: Offset(0, 4),
                    ),
                  ],
                ),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    // Ad ve Soyad TextFields
                    Text(
                      '• Adınız ve Soyadınız (TextField)',
                      style: TextStyle(
                        fontSize: 16,
                        fontWeight: FontWeight.w600,
                        color: Colors.grey[800],
                      ),
                    ),
                    SizedBox(height: 12),
                    
                    Row(
                      children: [
                        Expanded(
                          child: TextField(
                            controller: _nameController,
                            decoration: InputDecoration(
                              labelText: 'Adınız',
                              border: OutlineInputBorder(
                                borderRadius: BorderRadius.circular(8),
                              ),
                              prefixIcon: Icon(Icons.person),
                            ),
                          ),
                        ),
                        SizedBox(width: 12),
                        Expanded(
                          child: TextField(
                            controller: _surnameController,
                            decoration: InputDecoration(
                              labelText: 'Soyadınız',
                              border: OutlineInputBorder(
                                borderRadius: BorderRadius.circular(8),
                              ),
                              prefixIcon: Icon(Icons.person_outline),
                            ),
                          ),
                        ),
                      ],
                    ),
                    
                    SizedBox(height: 24),
                    
                    // Cinsiyet Dropdown
                    Text(
                      '• Cinsiyetinizi seçiniz (DropDown Button)',
                      style: TextStyle(
                        fontSize: 16,
                        fontWeight: FontWeight.w600,
                        color: Colors.grey[800],
                      ),
                    ),
                    SizedBox(height: 12),
                    
                    Container(
                      width: double.infinity,
                      padding: EdgeInsets.symmetric(horizontal: 12),
                      decoration: BoxDecoration(
                        border: Border.all(color: Colors.grey),
                        borderRadius: BorderRadius.circular(8),
                      ),
                      child: DropdownButtonHideUnderline(
                        child: DropdownButton<String>(
                          value: _selectedGender,
                          hint: Text('Cinsiyetinizi seçiniz'),
                          isExpanded: true,
                          items: _genders.map((String gender) {
                            return DropdownMenuItem<String>(
                              value: gender,
                              child: Text(gender),
                            );
                          }).toList(),
                          onChanged: (String? newValue) {
                            setState(() {
                              _selectedGender = newValue;
                            });
                          },
                        ),
                      ),
                    ),
                    
                    SizedBox(height: 24),
                    
                    // Hobiler Checkbox
                    Text(
                      '• Reşit misiniz? (Checkbox veya Checkbox Listile)',
                      style: TextStyle(
                        fontSize: 16,
                        fontWeight: FontWeight.w600,
                        color: Colors.grey[800],
                      ),
                    ),
                    SizedBox(height: 12),
                    
                    Text(
                      'Hobilerinizi seçiniz:',
                      style: TextStyle(
                        fontSize: 14,
                        fontWeight: FontWeight.w500,
                        color: Colors.grey[700],
                      ),
                    ),
                    SizedBox(height: 8),
                    
                    Wrap(
                      children: [
                        _buildCheckboxTile('Müzik', _music, (value) {
                          setState(() {
                            _music = value!;
                          });
                        }),
                        _buildCheckboxTile('Spor', _sports, (value) {
                          setState(() {
                            _sports = value!;
                          });
                        }),
                        _buildCheckboxTile('Okuma', _reading, (value) {
                          setState(() {
                            _reading = value!;
                          });
                        }),
                        _buildCheckboxTile('Seyahat', _travel, (value) {
                          setState(() {
                            _travel = value!;
                          });
                        }),
                        _buildCheckboxTile('Yemek Pişirme', _cooking, (value) {
                          setState(() {
                            _cooking = value!;
                          });
                        }),
                      ],
                    ),
                    
                    SizedBox(height: 24),
                    
                    // Sigara Switch
                    Text(
                      '• Sigara kullanıyor musunuz? (Switch listile)',
                      style: TextStyle(
                        fontSize: 16,
                        fontWeight: FontWeight.w600,
                        color: Colors.grey[800],
                      ),
                    ),
                    SizedBox(height: 12),
                    
                    Container(
                      padding: EdgeInsets.all(12),
                      decoration: BoxDecoration(
                        color: Colors.grey[50],
                        borderRadius: BorderRadius.circular(8),
                        border: Border.all(color: Colors.grey[300]!),
                      ),
                      child: Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          Text(
                            'Sigara kullanıyor musunuz?',
                            style: TextStyle(
                              fontSize: 14,
                              fontWeight: FontWeight.w500,
                            ),
                          ),
                          Switch(
                            value: _smokingStatus,
                            onChanged: (value) {
                              setState(() {
                                _smokingStatus = value;
                                if (!value) _cigarettesPerDay = 0;
                              });
                            },
                          ),
                        ],
                      ),
                    ),
                    
                    // Sigara Slider (eğer sigara kullanıyorsa)
                    if (_smokingStatus) ...[
                      SizedBox(height: 16),
                      Text(
                        '[Eğer evetse ⇒ Altta bir slider çıksın oradan günde kaç tane sigaranın kullanıldığı seçilebilsin]',
                        style: TextStyle(
                          fontSize: 12,
                          fontStyle: FontStyle.italic,
                          color: Colors.blue[600],
                        ),
                      ),
                      SizedBox(height: 8),
                      
                      Container(
                        padding: EdgeInsets.all(12),
                        decoration: BoxDecoration(
                          color: Colors.blue[50],
                          borderRadius: BorderRadius.circular(8),
                          border: Border.all(color: Colors.blue[200]!),
                        ),
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            Text(
                              'Günlük sigara adedi: ${_cigarettesPerDay.round()}',
                              style: TextStyle(
                                fontSize: 14,
                                fontWeight: FontWeight.w500,
                              ),
                            ),
                            Slider(
                              value: _cigarettesPerDay,
                              min: 0,
                              max: 40,
                              divisions: 40,
                              label: '${_cigarettesPerDay.round()} adet',
                              onChanged: (value) {
                                setState(() {
                                  _cigarettesPerDay = value;
                                });
                              },
                            ),
                          ],
                        ),
                      ),
                    ],
                    
                    SizedBox(height: 24),
                    
                    // Alt bilgi
                    Container(
                      padding: EdgeInsets.all(12),
                      decoration: BoxDecoration(
                        color: Colors.amber[50],
                        borderRadius: BorderRadius.circular(8),
                        border: Border.all(color: Colors.amber[200]!),
                      ),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Text(
                            '[Eğer hayırsa ⇒ slider çıkmasın]',
                            style: TextStyle(
                              fontSize: 12,
                              fontStyle: FontStyle.italic,
                              color: Colors.amber[700],
                            ),
                          ),
                          SizedBox(height: 8),
                          Text(
                            '- Bilgilerimi gönder butonu tıklandığında sayfanın en altında derste yaptığımız gibi bir container çıksın ve bilgiler orada gözüksün',
                            style: TextStyle(
                              fontSize: 12,
                              color: Colors.amber[800],
                            ),
                          ),
                        ],
                      ),
                    ),
                    
                    SizedBox(height: 20),
                    
                    // Gönder butonu
                    SizedBox(
                      width: double.infinity,
                      height: 50,
                      child: ElevatedButton(
                        onPressed: _submitForm,
                        style: ElevatedButton.styleFrom(
                          backgroundColor: Colors.blue[700],
                          foregroundColor: Colors.white,
                          shape: RoundedRectangleBorder(
                            borderRadius: BorderRadius.circular(8),
                          ),
                        ),
                        child: Text(
                          'Bilgilerimi Gönder',
                          style: TextStyle(
                            fontSize: 16,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                      ),
                    ),
                  ],
                ),
              ),
              
              SizedBox(height: 20),
              
              // Alt bilgi
              Container(
                padding: EdgeInsets.all(16),
                decoration: BoxDecoration(
                  color: Colors.grey[100],
                  borderRadius: BorderRadius.circular(8),
                ),
                child: Column(
                  children: [
                    Text(
                      'Uygulamanın tasarımını tamamen size bırakıyorum renk ve dizilik kısmını isterseniz row ve column widget\'larından yararlanabilirsiniz.',
                      style: TextStyle(
                        fontSize: 13,
                        color: Colors.grey[600],
                      ),
                      textAlign: TextAlign.center,
                    ),
                    SizedBox(height: 8),
                    Text(
                      'Aşağıda kendim yaptığım örnek bir tasarım var. Bu tasarımdan da örnek alabilirsiniz:',
                      style: TextStyle(
                        fontSize: 13,
                        fontWeight: FontWeight.w500,
                        color: Colors.grey[700],
                      ),
                      textAlign: TextAlign.center,
                    ),
                  ],
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildCheckboxTile(String title, bool value, Function(bool?) onChanged) {
    return Container(
      width: MediaQuery.of(context).size.width * 0.45,
      child: CheckboxListTile(
        title: Text(
          title,
          style: TextStyle(fontSize: 13),
        ),
        value: value,
        onChanged: onChanged,
        controlAffinity: ListTileControlAffinity.leading,
        dense: true,
      ),
    );
  }
}
