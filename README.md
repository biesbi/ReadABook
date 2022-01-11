/*  class MainActivity : AppCompatActivity() {
    lateinit var etIsim: EditText
    lateinit var rgCinsiyet: RadioGroup
    lateinit var rbErkek: RadioButton
    lateinit var rbKadin: RadioButton
    lateinit var etYas: EditText
    lateinit var etMaas: EditText
    lateinit var btnKaydet: Button
    lateinit var btnDuzenle: Button
    lateinit var btnSil: Button
    lateinit var btnListele: Button
    lateinit var ibArama: ImageButton
    lateinit var ibMesaj: ImageButton
    lateinit var ibEMail: ImageButton
    lateinit var lvListe: ListView
    lateinit var spListe: Spinner
    lateinit var tvMaas: TextView
    lateinit var tvYas: TextView
    var esasId = 0
    var esasYas =0
    var esasMaas =0
    var cns=""
    var egtDrm=""
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        etIsim=findViewById(R.id.etisim)
        etMaas=findViewById(R.id.etMaas)
        etYas=findViewById(R.id.etYas)
        rgCinsiyet=findViewById(R.id.rgCinsiyet)
        rbErkek=findViewById(R.id.rbErkek)
        rbKadin=findViewById(R.id.rbKadin)
        btnKaydet=findViewById(R.id.btnKaydet)
        btnDuzenle=findViewById(R.id.btnDuzenle)
        btnSil=findViewById(R.id.btnSil)
        btnListele=findViewById(R.id.btnListele)
        //ibArama = findViewById(R.id.ibArama)
        //ibMesaj = findViewById(R.id.ibMesaj)
        //ibEMail = findViewById(R.id.ibEMail)
        lvListe = findViewById(R.id.lvListe)
        spListe = findViewById(R.id.spListe)
        tvMaas = findViewById(R.id.tvMaas)
        tvYas = findViewById(R.id.tvYas)

        rgCinsiyet.setOnCheckedChangeListener { group, i ->
            if (i==R.id.rbErkek){
                cns=rbErkek.text.toString()
            }
            if (i==R.id.rbKadin){
                cns=rbKadin.text.toString()
            }
        }
        btnListele.setOnClickListener{
            listele()
        }
        btnKaydet.setOnClickListener {
            egtDrm=spListe.selectedItem.toString()
            val gelenisim = etIsim.text.toString()
            val gelenCinsiyet = cns.toString()
            val gelenEgitimDurumu = egtDrm.toString()
            val gelenYas = etYas.text.toString().toInt()
            val gelenMaas = etMaas.text.toString().toInt()
            val vt = VeriTabani(this@MainActivity)
            vt.veriEkle(gelenisim, gelenCinsiyet, gelenEgitimDurumu, gelenYas, gelenMaas)
            temizle()
            Toast.makeText(this, "Kayıt yapıldı", Toast.LENGTH_LONG).show()
            listele()
        }
        btnDuzenle.setOnClickListener {
            val gelenisim = etIsim.text.toString()
            val gelenCinsiyet = cns.toString()
            val gelenEgitimDurumu = egtDrm.toString()
            val gelenYas = etYas.text.toString().toInt()
            val gelenMaas = etMaas.text.toString().toInt()
            val vt = VeriTabani(this@MainActivity)
            vt.veriDuzenle(esasId,gelenisim, gelenCinsiyet, gelenEgitimDurumu, gelenYas, gelenMaas)
            temizle()
            Toast.makeText(this, "Kayıt Düzeltildi", Toast.LENGTH_LONG).show()
            listele()
        }
        btnSil.setOnClickListener {
            val vt = VeriTabani(this@MainActivity)
            vt.veriSil(esasId)
            Toast.makeText(this, "Kayıt Silindi", Toast.LENGTH_LONG).show()
            listele()
            temizle()
        }
    }
    private fun listele() {
        val vt = VeriTabani(this@MainActivity)
        val listeElemanları = vt.Listele()
        val adapter =
            ArrayAdapter<String>(this, android.R.layout.simple_list_item_1, listeElemanları)
        lvListe.adapter = adapter
        lvListe.setOnItemClickListener { parent, view, position, id ->
            var veri = lvListe.getItemAtPosition(position).toString()
            val veriBol = veri.split("-")
            esasId = Integer.parseInt(veriBol[0].toString())
            etIsim.setText(veriBol[1].toString())
            cns = veriBol[2].toString()
            egtDrm = veriBol[3].toString()
            etYas.setText(veriBol[4])
            etMaas.setText(veriBol[5])
        }
    }

    private fun temizle() {
        etIsim.text = null
        etYas.text = null
        etMaas.text = null
    }
}/*
//------------------------------------------------------------------------------------------
/*import android.content.ContentValues
import android.content.Context
import android.database.sqlite.SQLiteDatabase
import android.database.sqlite.SQLiteOpenHelper
import java.lang.Exception

private val DATABASE_NAME = "Kullanicilar.db"
private val DATABASE_VERSION = 1
private val TABLO_KISILER = "kisiler"
private val ROW_ID = "id"
private val ROW_AD = "ad"
private val ROW_CINSIYET = "cinsiyet"
private val ROW_EGITIM = "egitim"
private val ROW_YAS = "yas"
private val ROW_MAAS = "maas"

class VeriTabani(context: Context):
        SQLiteOpenHelper(context, DATABASE_NAME,null, DATABASE_VERSION){
        override fun onCreate(db: SQLiteDatabase?){
        db?.execSQL("CREATE TABLE "+ TABLO_KISILER + "("
                + ROW_ID + " INTEGER PRIMARY KEY AUTOINCREMENT, "
                + ROW_AD + " TEXT NOT NULL, "
                + ROW_CINSIYET + " TEXT NOT NULL, "
                + ROW_EGITIM + " TEXT NOT NULL, "
                + ROW_YAS + " INTEGER NOT NULL, "
                + ROW_MAAS + " INTEGER NOT NULL)")
        }

        override fun onUpgrade(db: SQLiteDatabase?, oldVersion: Int, newVersion: Int) {
                db?.execSQL("DROP TABLE IF EXISTS $TABLO_KISILER")
                onCreate(db)
        }
        fun veriEkle(ad: String,cinsiyet: String,egitim: String,yas: Int,maas: Int){
                val db=this.writableDatabase
                try {
                        val cv= ContentValues()
                        cv.put(ROW_AD,ad)
                        cv.put(ROW_CINSIYET,cinsiyet)
                        cv.put(ROW_EGITIM,egitim)
                        cv.put(ROW_YAS,yas)
                        cv.put(ROW_MAAS,maas)
                        db.insert(TABLO_KISILER, null, cv)
                } catch (e: Exception) {

                } finally {
                        db.close()
                }
        }
        fun Listele(): List<String> {
                val veriler = ArrayList<String>()
                val db = this.readableDatabase
                try {
                        val sutunlar = arrayOf(ROW_ID, ROW_AD, ROW_CINSIYET, ROW_EGITIM, ROW_YAS, ROW_MAAS)
                        val cursor = db.query(TABLO_KISILER, sutunlar, null, null, null, null, null, null)
                        while (cursor.moveToNext()) {
                                veriler.add(cursor.getInt(0).toString()
                                        + "-" + cursor.getString(1)
                                        + "-" + cursor.getString(2)
                                        + "-" + cursor.getString(3)
                                        + "-" + cursor.getInt(4).toString()
                                        + "-" + cursor.getInt(5).toString()
                                )
                        }
                } catch (e: Exception) {

                } finally {
                        db.close()
                }
                return veriler
        }
        fun veriDuzenle(id:Int, ad: String,cinsiyet: String,egitim: String,yas: Int,maas: Int) {
                val db = this.writableDatabase
                try {
                        val cv = ContentValues()
                        cv.put(ROW_AD,ad)
                        cv.put(ROW_CINSIYET,cinsiyet)
                        cv.put(ROW_EGITIM,egitim)
                        cv.put(ROW_YAS,yas)
                        cv.put(ROW_MAAS,maas)
                        val aramaCumlesi = "$ROW_ID = $id"
                        db.update(TABLO_KISILER,  cv,aramaCumlesi,null)
                } catch (e: Exception) {
                } finally {
                        db.close()
                }
        }
        fun veriSil(id:Int) {
                val db = this.writableDatabase
                try {
                        val aramaCumlesi = "$ROW_ID = $id"
                        db.delete(TABLO_KISILER, aramaCumlesi,null)
                } catch (e: Exception) {
                } finally {
                        db.close()
                }
        }
}/*
