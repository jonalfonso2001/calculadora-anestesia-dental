# calculadora-anestesia-dental
Scrip interactivo en Python para el calculo de dosificacion de anestesicos locales
#!/usr/bin/env python3
"""
Calculadora de dosis de lidocaína 2% (mejorada con validaciones).
"""

def pedir_opcion_paciente():
    while True:
        print("Tipo de paciente:")
        print("1. Adulto")
        print("2. Pediátrico (Niño)")
        opcion = input("Selecciona una opción (1 o 2): ").strip()
        if opcion in ("1", "2"):
            return opcion
        print("Opción no válida. Intenta de nuevo.\n")

def pedir_peso():
    while True:
        raw = input("Ingresa el peso del paciente (kg): ").strip().replace(",", ".")
        try:
            peso = float(raw)
            if peso <= 0:
                print("El peso debe ser un número positivo. Intenta de nuevo.")
                continue
            return peso
        except ValueError:
            print("Entrada inválida. Ingresa un número (por ejemplo: 70 o 12.5).")

def pedir_compromiso_cardio():
    while True:
        resp = input("\n¿El paciente presenta compromiso cardiovascular? (Hipertensión, cardiopatías congénitas, etc.) Responde (SI / NO): ").strip().upper()
        if resp in ("SI", "S"):
            return True
        if resp in ("NO", "N"):
            return False
        print("Respuesta no válida. Usa SI o NO.")

def calcular_anestesia_completa():
    print("==================================================")
    print("  SISTEMA DE DOSIFICACIÓN CLÍNICA: LIDOCAÍNA 2%  ")
    print("==================================================")
    
    opcion_paciente = pedir_opcion_paciente()
    peso = pedir_peso()
    compromiso_cardio = pedir_compromiso_cardio()
    
    # Parámetros
    mg_por_cartucho = 36.0  # mg por cartucho (1.8 mL al 2% = 36 mg)
    
    # Inicializar variables
    dosis_final_mg = 0.0
    cartuchos_maximos = 0.0
    nota = ""
    
    if opcion_paciente == "1":
        # Adulto
        limite_absoluto_adulto = 300.0  # mg
        if compromiso_cardio:
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
        # Pediátrico
        limite_absoluto_nino = 150.0  # mg
        if compromiso_cardio:
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

    # Presentación de resultados
    cartuchos_display = round(cartuchos_maximos, 1)
    cartuchos_completos = int(cartuchos_maximos)  # número entero de cartuchos completos (si hace falta)
    
    print(f"\n==================================================")
    print(f"               REPORTE DE SEGURIDAD               ")
    print(f"==================================================")
    print(f"- Tipo de Paciente: {'Adulto' if opcion_paciente == '1' else 'Pediátrico'}")
    print(f"- Peso registrado: {peso:.2f} kg")
    print(f"- Dosis máxima permitida: {dosis_final_mg:.2f} mg")
    print(f"- Cantidad máxima segura (aprox.): {cartuchos_display} cartucho(s)")
    print(f"- Cartuchos completos (enteros): {cartuchos_completos} cartucho(s)")
    print(f"--------------------------------------------------")
    print(f"{nota}")
    print(f"==================================================")

if __name__ == "__main__":
    calcular_anestesia_completa()
