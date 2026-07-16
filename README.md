# calculadora-anestesia-dental
Scrip interactivo en Python para el calculo de dosificacion de anestesicos locales (lidocaina)
def calcular_anestesia_completa():
    print("==================================================")
    print("  SISTEMA DE DOSIFICACIÓN CLÍNICA: LIDOCAÍNA 2%  ")
    print("==================================================")
    
    # 1. Selección del tipo de paciente
    print("Tipo de paciente:")
    print("1. Adulto")
    print("2. Pediátrico (Niño)")
    opcion_paciente = input("Selecciona una opción (1 o 2): ").strip()
    
    if opcion_paciente not in ["1", "2"]:
        print("Opción no válida. Reinicia el programa.")
        return

    # 2. Datos básicos del paciente
    peso = float(input("Ingresa el peso del paciente (kg): "))
    
    print("\n¿El paciente presenta compromiso cardiovascular?")
    print("(Hipertensión, cardiopatías congénitas, etc.)")
    compromiso_cardio = input("Responde (SI / NO): ").strip().upper()
    
    # 3. Parámetros fijos
    mg_por_cartucho = 36
    
    # 4. Lógica Clínica según el Tipo de Paciente
    if opcion_paciente == "1":
        # --- LÓGICA PARA ADULTOS ---
        limite_absoluto_adulto = 300  # Máximo 300 mg
        
        if compromiso_cardio == "SI":
            cartuchos_maximos = 2.0
            dosis_final_mg = cartuchos_maximos * mg_por_cartucho
            nota = "ALERTA: Adulto con compromiso cardiovascular. Dosis limitada por vasoconstrictor (Máx. 2 cartuchos / 0.04 mg epinefrina)."
        else:
            dosis_por_peso = peso * 4.4
            if dosis_por_peso > limite_absoluto_adulto:
                dosis_final_mg = limite_absoluto_adulto
                nota = "Nota: Adulto sano. Se aplicó el límite máximo absoluto de 300 mg por seguridad."
            else:
                dosis_final_mg = dosis_por_peso
                nota = "Nota: Adulto sano. Dosis calculada con base en el peso corporal."
            cartuchos_maximos = dosis_final_mg / mg_por_cartucho

    else:
        # --- LÓGICA PEDIÁTRICA ---
        limite_absoluto_nino = 150  # Máximo absoluto estricto en niños (150 mg)
        
        if compromiso_cardio == "SI":
            # En niños con cardiopatías la dosis de vasoconstrictor es extremadamente baja
            cartuchos_maximos = 1.0
            dosis_final_mg = cartuchos_maximos * mg_por_cartucho
            nota = "ALERTA MÁXIMA: Paciente pediátrico con compromiso cardiovascular. Dosis restringida a 1 cartucho por seguridad."
        else:
            dosis_por_peso = peso * 4.4
            if dosis_por_peso > limite_absoluto_nino:
                dosis_final_mg = limite_absoluto_nino
                nota = "Nota: Paciente pediátrico. Se aplicó el límite máximo absoluto infantil de 150 mg."
            else:
                dosis_final_mg = dosis_por_peso
                nota = "Nota: Paciente pediátrico. Dosis calculada con base en el peso corporal."
            cartuchos_maximos = dosis_final_mg / mg_por_cartucho

    # 5. Despliegue de resultados médicos
    print(f"\n==================================================")
    print(f"               REPORTE DE SEGURIDAD               ")
    print(f"==================================================")
    print(f"- Tipo de Paciente: {'Adulto' if opcion_paciente == '1' else 'Pediátrico'}")
    print(f"- Peso registrado: {peso} kg")
    print(f"- Dosis máxima permitida: {round(dosis_final_mg, 2)} mg")
    print(f"- Cantidad máxima segura: {round(cartuchos_maximos, 1)} cartuchos.")
    print(f"--------------------------------------------------")
    print(f"{nota}")
    print(f"==================================================")

# Ejecutar el programa
calcular_anestesia_completa()
