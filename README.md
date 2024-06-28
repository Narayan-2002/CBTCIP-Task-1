package com.example.unitconverter;

import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Spinner;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private EditText editTextInput;
    private Spinner spinnerFromUnit, spinnerToUnit;
    private TextView textViewResult;
    private Button buttonConvert;

    private String[] units = {"Meter", "Centimeter", "Kilometer", "Gram", "Kilogram", "Foot", "Inch"};

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        editTextInput = findViewById(R.id.editTextInput);
        spinnerFromUnit = findViewById(R.id.spinnerFromUnit);
        spinnerToUnit = findViewById(R.id.spinnerToUnit);
        textViewResult = findViewById(R.id.textViewResult);
        buttonConvert = findViewById(R.id.buttonConvert);

        ArrayAdapter<String> adapter = new ArrayAdapter<>(this, android.R.layout.simple_spinner_item, units);
        adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
        spinnerFromUnit.setAdapter(adapter);
        spinnerToUnit.setAdapter(adapter);

        buttonConvert.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                convertUnits();
            }
        });
    }

    private void convertUnits() {
        String input = editTextInput.getText().toString();
        if (input.isEmpty()) {
            textViewResult.setText("Please enter a value");
            return;
        }

        double inputValue = Double.parseDouble(input);
        String fromUnit = spinnerFromUnit.getSelectedItem().toString();
        String toUnit = spinnerToUnit.getSelectedItem().toString();

        double result = convert(inputValue, fromUnit, toUnit);
        textViewResult.setText(String.format("%.2f", result) + " " + toUnit);
    }

    private double convert(double value, String fromUnit, String toUnit) {
        double result = 0.0;

        // Convert the input value to meters/grams for uniform conversion
        switch (fromUnit) {
            case "Meter":
                result = value;
                break;
            case "Centimeter":
                result = value / 100;
                break;
            case "Kilometer":
                result = value * 1000;
                break;
            case "Gram":
                result = value;
                break;
            case "Kilogram":
                result = value * 1000;
                break;
            case "Foot":
                result = value * 0.3048;
                break;
            case "Inch":
                result = value * 0.0254;
                break;
        }

        // Convert the value from meters/grams to the target unit
        switch (toUnit) {
            case "Meter":
                break;
            case "Centimeter":
                result = result * 100;
                break;
            case "Kilometer":
                result = result / 1000;
                break;
            case "Gram":
                break;
            case "Kilogram":
                result = result / 1000;
                break;
            case "Foot":
                result = result / 0.3048;
                break;
            case "Inch":
                result = result / 0.0254;
                break;
        }

        return result;
    }
}
