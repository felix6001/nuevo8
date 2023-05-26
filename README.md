# nuevo8
implementation 'com.google.android.gms:play-services-maps:17.0.0'
class MainActivity : AppCompatActivity() {

    private lateinit var placesMenu: ListView
    private val places = arrayOf("Lugar 1", "Lugar 2", "Lugar 3", "Lugar 4")
    private val locations = arrayOf(
        LatLng(40.7128, -74.0060), // Coordenadas del Lugar 1
        LatLng(40.7590, -73.9845), // Coordenadas del Lugar 2
        LatLng(40.7484, -73.9857), // Coordenadas del Lugar 3
        LatLng(40.7060, -74.0088) // Coordenadas del Lugar 4
    )

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        placesMenu = findViewById(R.id.places_menu)
        val adapter = ArrayAdapter(this, android.R.layout.simple_list_item_1, places)
        placesMenu.adapter = adapter

        placesMenu.setOnItemClickListener { _, _, position, _ ->
            val selectedLocation = locations[position]
            val intent = Intent(this, MapsActivity::class.java)
            intent.putExtra("location", selectedLocation)
            startActivity(intent)
        }
    }
}
class MapsActivity : AppCompatActivity(), OnMapReadyCallback {

    private lateinit var map: GoogleMap
    private lateinit var selectedLocation: LatLng

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_maps)

        selectedLocation = intent.getParcelableExtra("location")

        val mapFragment = supportFragmentManager.findFragmentById(R.id.map_fragment) as SupportMapFragment
        mapFragment.getMapAsync(this)
    }

    override fun onMapReady(googleMap: GoogleMap) {
        map = googleMap

        val markerOptions = MarkerOptions()
            .position(selectedLocation)
            .icon(BitmapDescriptorFactory.fromResource(R.drawable.custom_marker))

        map.addMarker(markerOptions)
        map.moveCamera(CameraUpdateFactory.newLatLngZoom(selectedLocation, 12f))
    }
}
<bitmap xmlns:android="http://schemas.android.com/apk/res/android"
    android:src="@drawable/ic_custom_marker"
    android:gravity="center" />
