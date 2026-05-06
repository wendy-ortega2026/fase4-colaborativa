"""
Versión base principal elaborada por Diego.
Se deja una sección pequeña para que otro compañero realice un aporte menor con su propio commit.

Proyecto Fase 4 - Programación 213023
Sistema Integral de Gestión de Clientes, Servicios y Reservas
Empresa: Software FJ

Este archivo único contiene todo el desarrollo solicitado:
- Programación orientada a objetos.
- Clase abstracta general.
- Clase Cliente con encapsulación y validaciones.
- Clase abstracta Servicio.
- Tres servicios especializados.
- Clase Reserva.
- Excepciones personalizadas.
- Manejo de try/except, try/except/else, try/except/finally.
- Encadenamiento de excepciones.
- Registro de eventos y errores en archivo de logs.
- Simulación de más de 10 operaciones válidas e inválidas.
- Sin uso de bases de datos.
"""

from abc import ABC, abstractmethod
from datetime import datetime
import logging
import os
import re
from typing import Optional


# ============================================================
# CONFIGURACIÓN DEL ARCHIVO DE LOGS
# ============================================================

# Se crea la carpeta logs si no existe.
os.makedirs("logs", exist_ok=True)

# Se configura el archivo donde se guardarán eventos y errores.
logging.basicConfig(
    filename="logs/software_fj.log",
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s",
    encoding="utf-8"
)


# ============================================================
# EXCEPCIONES PERSONALIZADAS DEL SISTEMA
# ============================================================

class ErrorSistema(Exception):
    """Excepción base para errores controlados del sistema."""
    pass


class ErrorValidacion(ErrorSistema):
    """Se lanza cuando un dato ingresado no cumple las validaciones."""
    pass


class ErrorServicioNoDisponible(ErrorSistema):
    """Se lanza cuando un servicio no se encuentra disponible."""
    pass


class ErrorReserva(ErrorSistema):
    """Se lanza cuando ocurre un problema en una reserva."""
    pass


class ErrorCalculoCosto(ErrorSistema):
    """Se lanza cuando no es posible calcular correctamente un costo."""
    pass


# ============================================================
# CLASE ABSTRACTA GENERAL
# ============================================================

class EntidadSistema(ABC):
    """
    Clase abstracta general para representar entidades del sistema.
    Todas las entidades principales tienen un identificador y deben
    implementar el método mostrar_informacion().
    """

    def __init__(self, identificador: str):
        # Se valida que el identificador no esté vacío.
        if not identificador or not str(identificador).strip():
            raise ErrorValidacion("El identificador no puede estar vacío.")

        # Se guarda el identificador como atributo protegido.
        self._identificador = str(identificador).strip()

    @property
    def identificador(self):
        """Devuelve el identificador de la entidad."""
        return self._identificador

    @abstractmethod
    def mostrar_informacion(self):
        """Método abstracto que debe implementar cada clase hija."""
        pass


# ============================================================
# CLASE CLIENTE
# ============================================================

class Cliente(EntidadSistema):
    """
    Clase que representa un cliente de Software FJ.
    Aplica encapsulación usando atributos privados y propiedades.
    """

    def __init__(self, identificador: str, nombre: str, correo: str, telefono: str):
        # Se inicializa la clase padre.
        super().__init__(identificador)

        # Se declaran atributos privados.
        self.__nombre = None
        self.__correo = None
        self.__telefono = None

        # Se asignan valores usando setters para aplicar validaciones.
        self.nombre = nombre
        self.correo = correo
        self.telefono = telefono

        # Se registra el evento en logs.
        logging.info(f"Cliente creado correctamente: {self.__nombre}")

    @property
    def nombre(self):
        """Devuelve el nombre del cliente."""
        return self.__nombre

    @nombre.setter
    def nombre(self, valor):
        """Valida y asigna el nombre del cliente."""
        if not valor or len(valor.strip()) < 3:
            raise ErrorValidacion("El nombre del cliente debe tener mínimo 3 caracteres.")
        self.__nombre = valor.strip().title()

    @property
    def correo(self):
        """Devuelve el correo del cliente."""
        return self.__correo

    @correo.setter
    def correo(self, valor):
        """Valida y asigna el correo del cliente."""
        patron = r"^[\w\.-]+@[\w\.-]+\.\w+$"
        if not valor or not re.match(patron, valor.strip()):
            raise ErrorValidacion("El correo del cliente no tiene un formato válido.")
        self.__correo = valor.strip().lower()

    @property
    def telefono(self):
        """Devuelve el teléfono del cliente."""
        return self.__telefono

    @telefono.setter
    def telefono(self, valor):
        """Valida y asigna el teléfono del cliente."""
        if not valor or not str(valor).isdigit() or len(str(valor)) < 7:
            raise ErrorValidacion("El teléfono debe contener solo números y mínimo 7 dígitos.")
        self.__telefono = str(valor)

    def mostrar_informacion(self):
        """Muestra la información básica del cliente."""
        return (
            f"Cliente: {self.__nombre} | "
            f"Documento: {self.identificador} | "
            f"Correo: {self.__correo} | "
            f"Teléfono: {self.__telefono}"
        )


# ============================================================
# CLASE ABSTRACTA SERVICIO
# ============================================================

class Servicio(EntidadSistema):
    """
    Clase abstracta para representar servicios ofrecidos por Software FJ.
    Las clases hijas deben calcular costo, describir servicio y validar parámetros.
    """

    def __init__(self, identificador: str, nombre: str, tarifa_base: float, disponible: bool = True):
        # Se inicializa la clase padre.
        super().__init__(identificador)

        # Se valida el nombre del servicio.
        if not nombre or len(nombre.strip()) < 3:
            raise ErrorValidacion("El nombre del servicio debe tener mínimo 3 caracteres.")

        # Se valida la tarifa base.
        if tarifa_base <= 0:
            raise ErrorValidacion("La tarifa base del servicio debe ser mayor que cero.")

        # Se asignan atributos protegidos.
        self._nombre = nombre.strip().title()
        self._tarifa_base = float(tarifa_base)
        self._disponible = disponible

        # Se registra el evento.
        logging.info(f"Servicio creado: {self._nombre}")

    @property
    def nombre(self):
        """Devuelve el nombre del servicio."""
        return self._nombre

    @property
    def disponible(self):
        """Devuelve si el servicio está disponible."""
        return self._disponible

    def cambiar_disponibilidad(self, disponible: bool):
        """Permite activar o desactivar el servicio."""
        self._disponible = bool(disponible)
        logging.info(f"Disponibilidad actualizada para {self._nombre}: {self._disponible}")

    def calcular_costo_con_opciones(
        self,
        duracion: float,
        impuesto: float = 0.0,
        descuento: float = 0.0
    ):
        """
        Simula sobrecarga de métodos mediante parámetros opcionales.
        Permite calcular el costo normal, con impuesto, con descuento,
        o con impuesto y descuento al mismo tiempo.
        """
        costo_base = self.calcular_costo(duracion)

        # Se valida el impuesto.
        if impuesto < 0:
            raise ErrorCalculoCosto("El impuesto no puede ser negativo.")

        # Se valida el descuento.
        if descuento < 0 or descuento > 1:
            raise ErrorCalculoCosto("El descuento debe estar entre 0 y 1.")

        # Se calcula el costo final.
        costo_con_descuento = costo_base * (1 - descuento)
        costo_final = costo_con_descuento * (1 + impuesto)

        return round(costo_final, 2)

    @abstractmethod
    def calcular_costo(self, duracion: float):
        """Calcula el costo del servicio."""
        pass

    @abstractmethod
    def describir_servicio(self):
        """Describe el servicio."""
        pass

    @abstractmethod
    def validar_parametros(self, duracion: float):
        """Valida parámetros específicos del servicio."""
        pass

    def mostrar_informacion(self):
        """Muestra la información general del servicio."""
        estado = "Disponible" if self._disponible else "No disponible"
        return (
            f"Servicio: {self._nombre} | "
            f"Código: {self.identificador} | "
            f"Tarifa base: ${self._tarifa_base:,.0f} | "
            f"Estado: {estado}"
        )


# ============================================================
# SERVICIO 1: RESERVA DE SALA
# ============================================================

class ReservaSala(Servicio):
    """Servicio especializado para reserva de salas."""

    def __init__(self, identificador: str, nombre: str, tarifa_base: float, capacidad: int, disponible: bool = True):
        # Se inicializa la clase padre.
        super().__init__(identificador, nombre, tarifa_base, disponible)

        # Se valida la capacidad.
        if capacidad <= 0:
            raise ErrorValidacion("La capacidad de la sala debe ser mayor que cero.")

        # Se asigna la capacidad.
        self.capacidad = capacidad

    def validar_parametros(self, duracion: float):
        """Valida que la duración de la reserva de sala sea correcta."""
        if duracion <= 0:
            raise ErrorReserva("La duración de la reserva de sala debe ser mayor que cero.")
        if duracion > 8:
            raise ErrorReserva("Una sala no puede reservarse por más de 8 horas continuas.")

    def calcular_costo(self, duracion: float):
        """Calcula el costo de la reserva de sala."""
        self.validar_parametros(duracion)
        return round(self._tarifa_base * duracion, 2)

    def describir_servicio(self):
        """Describe el servicio de sala."""
        return f"Reserva de sala con capacidad para {self.capacidad} personas."


# ============================================================
# SERVICIO 2: ALQUILER DE EQUIPO
# ============================================================

class AlquilerEquipo(Servicio):
    """Servicio especializado para alquiler de equipos."""

    def __init__(self, identificador: str, nombre: str, tarifa_base: float, tipo_equipo: str, disponible: bool = True):
        # Se inicializa la clase padre.
        super().__init__(identificador, nombre, tarifa_base, disponible)

        # Se valida el tipo de equipo.
        if not tipo_equipo or len(tipo_equipo.strip()) < 3:
            raise ErrorValidacion("El tipo de equipo debe tener mínimo 3 caracteres.")

        # Se asigna el tipo de equipo.
        self.tipo_equipo = tipo_equipo.strip().title()

    def validar_parametros(self, duracion: float):
        """Valida que la duración del alquiler sea correcta."""
        if duracion <= 0:
            raise ErrorReserva("La duración del alquiler debe ser mayor que cero.")
        if duracion > 30:
            raise ErrorReserva("El alquiler de equipo no puede superar 30 días.")

    def calcular_costo(self, duracion: float):
        """Calcula el costo del alquiler de equipo."""
        self.validar_parametros(duracion)

        # Se agrega una garantía fija del 10%.
        garantia = self._tarifa_base * duracion * 0.10
        return round((self._tarifa_base * duracion) + garantia, 2)

    def describir_servicio(self):
        """Describe el servicio de alquiler."""
        return f"Alquiler de equipo tipo {self.tipo_equipo}."


# ============================================================
# SERVICIO 3: ASESORÍA ESPECIALIZADA
# ============================================================

class AsesoriaEspecializada(Servicio):
    """Servicio especializado para asesorías técnicas."""

    def __init__(self, identificador: str, nombre: str, tarifa_base: float, area: str, disponible: bool = True):
        # Se inicializa la clase padre.
        super().__init__(identificador, nombre, tarifa_base, disponible)

        # Se valida el área.
        if not area or len(area.strip()) < 3:
            raise ErrorValidacion("El área de asesoría debe tener mínimo 3 caracteres.")

        # Se asigna el área.
        self.area = area.strip().title()

    def validar_parametros(self, duracion: float):
        """Valida la duración de la asesoría."""
        if duracion <= 0:
            raise ErrorReserva("La duración de la asesoría debe ser mayor que cero.")
        if duracion > 6:
            raise ErrorReserva("Una asesoría no puede superar 6 horas por sesión.")

    def calcular_costo(self, duracion: float):
        """Calcula el costo de la asesoría especializada."""
        self.validar_parametros(duracion)

        # Si la asesoría dura más de 3 horas, se cobra un recargo del 15%.
        costo = self._tarifa_base * duracion
        if duracion > 3:
            costo = costo * 1.15

        return round(costo, 2)

    def describir_servicio(self):
        """Describe el servicio de asesoría."""
        return f"Asesoría especializada en el área de {self.area}."


# ============================================================
# CLASE RESERVA
# ============================================================

class Reserva(EntidadSistema):
    """
    Clase que integra cliente, servicio, duración y estado.
    Permite confirmar, cancelar y procesar reservas.
    """

    ESTADOS_VALIDOS = ["Creada", "Confirmada", "Cancelada", "Procesada"]

    def __init__(self, identificador: str, cliente: Cliente, servicio: Servicio, duracion: float):
        # Se inicializa la clase padre.
        super().__init__(identificador)

        # Se valida que el cliente sea una instancia válida.
        if not isinstance(cliente, Cliente):
            raise ErrorReserva("La reserva debe asociarse a un cliente válido.")

        # Se valida que el servicio sea una instancia válida.
        if not isinstance(servicio, Servicio):
            raise ErrorReserva("La reserva debe asociarse a un servicio válido.")

        # Se valida que el servicio esté disponible.
        if not servicio.disponible:
            raise ErrorServicioNoDisponible(f"El servicio {servicio.nombre} no se encuentra disponible.")

        # Se validan los parámetros del servicio.
        servicio.validar_parametros(duracion)

        # Se asignan los atributos de la reserva.
        self.cliente = cliente
        self.servicio = servicio
        self.duracion = duracion
        self.estado = "Creada"
        self.fecha_creacion = datetime.now()

        # Se registra el evento.
        logging.info(f"Reserva creada correctamente: {self.identificador}")

    def confirmar(self):
        """Confirma una reserva creada."""
        if self.estado != "Creada":
            raise ErrorReserva("Solo se pueden confirmar reservas en estado Creada.")

        self.estado = "Confirmada"
        logging.info(f"Reserva confirmada: {self.identificador}")

    def cancelar(self, motivo: Optional[str] = None):
        """Cancela una reserva que no haya sido procesada."""
        if self.estado == "Procesada":
            raise ErrorReserva("No se puede cancelar una reserva ya procesada.")

        self.estado = "Cancelada"
        logging.info(f"Reserva cancelada: {self.identificador}. Motivo: {motivo or 'No informado'}")

    def procesar(self, impuesto: float = 0.19, descuento: float = 0.0):
        """
        Procesa la reserva y calcula el costo final.
        Usa try/except/else/finally para demostrar manejo robusto.
        """
        try:
            # Se valida que la reserva esté confirmada.
            if self.estado != "Confirmada":
                raise ErrorReserva("La reserva debe estar confirmada antes de procesarse.")

            # Se calcula el costo final.
            costo_final = self.servicio.calcular_costo_con_opciones(
                self.duracion,
                impuesto=impuesto,
                descuento=descuento
            )

        except ErrorSistema as error:
            # Se registra el error.
            logging.error(f"Error al procesar reserva {self.identificador}: {error}")

            # Se relanza el error para manejarlo en la simulación.
            raise

        else:
            # Se actualiza el estado si no hubo error.
            self.estado = "Procesada"

            # Se registra el procesamiento exitoso.
            logging.info(f"Reserva procesada correctamente: {self.identificador}. Costo: {costo_final}")

            return costo_final

        finally:
            # Este bloque siempre se ejecuta.
            logging.info(f"Finalizó intento de procesamiento para reserva: {self.identificador}")

    def mostrar_informacion(self):
        """Muestra la información de la reserva."""
        return (
            f"Reserva: {self.identificador} | "
            f"Cliente: {self.cliente.nombre} | "
            f"Servicio: {self.servicio.nombre} | "
            f"Duración: {self.duracion} | "
            f"Estado: {self.estado}"
        )


# ============================================================
# FUNCIONES DE APOYO PARA LA SIMULACIÓN
# ============================================================

def ejecutar_operacion(numero, descripcion, funcion):
    """
    Ejecuta una operación de prueba sin detener el programa.
    Esta función permite demostrar que el sistema continúa activo
    aunque se presenten errores.
    """
    print(f"\nOPERACIÓN {numero}: {descripcion}")
    print("-" * 70)

    try:
        # Se intenta ejecutar la función recibida como parámetro.
        resultado = funcion()

    except ErrorSistema as error:
        # Se capturan errores personalizados.
        print(f"ERROR CONTROLADO: {error}")
        logging.error(f"Operación {numero} fallida: {error}")

    except Exception as error:
        # Se capturan errores inesperados.
        print(f"ERROR INESPERADO: {error}")
        logging.exception(f"Operación {numero} generó un error inesperado.")

    else:
        # Se ejecuta cuando no ocurre ningún error.
        if resultado is not None:
            print(resultado)
        print("Operación ejecutada correctamente.")
        logging.info(f"Operación {numero} ejecutada correctamente.")

    finally:
        # Siempre se ejecuta.
        print("El sistema continúa funcionando.")


def crear_servicio_con_encadenamiento():
    """
    Demuestra encadenamiento de excepciones.
    Convierte un error técnico de Python en un error personalizado
    más claro para el usuario.
    """
    try:
        # Esta conversión fallará porque el texto no es numérico.
        tarifa = float("valor_no_numerico")

        # Esta línea no se ejecuta porque la conversión anterior falla.
        return ReservaSala("S004", "Sala con error", tarifa, 10)

    except ValueError as error_original:
        # Se encadena la excepción original con una excepción personalizada.
        raise ErrorValidacion(
            "No fue posible crear el servicio porque la tarifa no es numérica."
        ) from error_original




# ============================================================
# APORTE COLABORATIVO PENDIENTE
# ============================================================

def aporte_colaborativo_pendiente(clientes, servicios, reservas):
    """
    Esta sección queda reservada para que otro compañero haga un aporte pequeño
    mediante un commit propio.

    Instrucción para el compañero:
    - No borrar el código principal.
    - Agregar aquí una operación 16 o una validación sencilla.
    - Guardar el cambio con un commit propio en GitHub.

    Ejemplo de aporte posible:
    Crear una reserva adicional válida o fallida usando las listas clientes,
    servicios y reservas.
    """

    print("\nAPORTE COLABORATIVO PENDIENTE")
    print("-" * 70)
    print("Esta sección está reservada para que un compañero agregue un aporte menor.")
    print("El programa principal ya funciona correctamente.")


# ============================================================
# SIMULACIÓN PRINCIPAL DEL SISTEMA
# ============================================================

def main():
    """
    Función principal.
    Ejecuta operaciones válidas e inválidas para demostrar el sistema.
    """

    print("=" * 70)
    print("SISTEMA INTEGRAL DE GESTIÓN - SOFTWARE FJ")
    print("Fase 4 - Programación Orientada a Objetos y Excepciones")
    print("=" * 70)

    # Listas internas para manejar datos sin base de datos.
    clientes = []
    servicios = []
    reservas = []

    # ------------------------------------------------------------
    # OPERACIÓN 1: Registro válido de cliente.
    # ------------------------------------------------------------
    def op1():
        cliente = Cliente("1001", "Laura Martínez", "laura@email.com", "3001234567")
        clientes.append(cliente)
        return cliente.mostrar_informacion()

    ejecutar_operacion(1, "Registrar cliente válido", op1)

    # ------------------------------------------------------------
    # OPERACIÓN 2: Registro inválido de cliente por correo incorrecto.
    # ------------------------------------------------------------
    def op2():
        cliente = Cliente("1002", "Carlos Pérez", "correo_malo", "3007654321")
        clientes.append(cliente)
        return cliente.mostrar_informacion()

    ejecutar_operacion(2, "Registrar cliente con correo inválido", op2)

    # ------------------------------------------------------------
    # OPERACIÓN 3: Registro inválido de cliente por teléfono incorrecto.
    # ------------------------------------------------------------
    def op3():
        cliente = Cliente("1003", "Ana Gómez", "ana@email.com", "abc123")
        clientes.append(cliente)
        return cliente.mostrar_informacion()

    ejecutar_operacion(3, "Registrar cliente con teléfono inválido", op3)

    # ------------------------------------------------------------
    # OPERACIÓN 4: Crear servicio válido de reserva de sala.
    # ------------------------------------------------------------
    def op4():
        servicio = ReservaSala("S001", "Sala de juntas", 50000, capacidad=20)
        servicios.append(servicio)
        return servicio.mostrar_informacion() + "\n" + servicio.describir_servicio()

    ejecutar_operacion(4, "Crear servicio válido de reserva de sala", op4)

    # ------------------------------------------------------------
    # OPERACIÓN 5: Crear servicio válido de alquiler de equipo.
    # ------------------------------------------------------------
    def op5():
        servicio = AlquilerEquipo("S002", "Alquiler computador portátil", 80000, "Computador portátil")
        servicios.append(servicio)
        return servicio.mostrar_informacion() + "\n" + servicio.describir_servicio()

    ejecutar_operacion(5, "Crear servicio válido de alquiler de equipo", op5)

    # ------------------------------------------------------------
    # OPERACIÓN 6: Crear servicio válido de asesoría especializada.
    # ------------------------------------------------------------
    def op6():
        servicio = AsesoriaEspecializada("S003", "Asesoría en software", 120000, "Desarrollo de software")
        servicios.append(servicio)
        return servicio.mostrar_informacion() + "\n" + servicio.describir_servicio()

    ejecutar_operacion(6, "Crear servicio válido de asesoría especializada", op6)

    # ------------------------------------------------------------
    # OPERACIÓN 7: Crear servicio inválido por tarifa negativa.
    # ------------------------------------------------------------
    def op7():
        servicio = ReservaSala("S004", "Sala económica", -10000, capacidad=10)
        servicios.append(servicio)
        return servicio.mostrar_informacion()

    ejecutar_operacion(7, "Crear servicio con tarifa negativa", op7)

    # ------------------------------------------------------------
    # OPERACIÓN 8: Crear servicio con error técnico encadenado.
    # ------------------------------------------------------------
    def op8():
        servicio = crear_servicio_con_encadenamiento()
        servicios.append(servicio)
        return servicio.mostrar_informacion()

    ejecutar_operacion(8, "Crear servicio con tarifa no numérica y excepción encadenada", op8)

    # ------------------------------------------------------------
    # OPERACIÓN 9: Crear reserva válida.
    # ------------------------------------------------------------
    def op9():
        reserva = Reserva("R001", clientes[0], servicios[0], duracion=3)
        reservas.append(reserva)
        return reserva.mostrar_informacion()

    ejecutar_operacion(9, "Crear reserva válida de sala", op9)

    # ------------------------------------------------------------
    # OPERACIÓN 10: Confirmar reserva válida.
    # ------------------------------------------------------------
    def op10():
        reservas[0].confirmar()
        return reservas[0].mostrar_informacion()

    ejecutar_operacion(10, "Confirmar reserva creada", op10)

    # ------------------------------------------------------------
    # OPERACIÓN 11: Procesar reserva con impuesto y descuento.
    # ------------------------------------------------------------
    def op11():
        costo = reservas[0].procesar(impuesto=0.19, descuento=0.10)
        return f"Reserva procesada. Costo final con IVA y descuento: ${costo:,.0f}"

    ejecutar_operacion(11, "Procesar reserva con impuesto y descuento", op11)

    # ------------------------------------------------------------
    # OPERACIÓN 12: Intentar cancelar reserva ya procesada.
    # ------------------------------------------------------------
    def op12():
        reservas[0].cancelar("El cliente cambió de opinión")
        return reservas[0].mostrar_informacion()

    ejecutar_operacion(12, "Intentar cancelar reserva ya procesada", op12)

    # ------------------------------------------------------------
    # OPERACIÓN 13: Crear reserva fallida por duración excesiva.
    # ------------------------------------------------------------
    def op13():
        reserva = Reserva("R002", clientes[0], servicios[0], duracion=12)
        reservas.append(reserva)
        return reserva.mostrar_informacion()

    ejecutar_operacion(13, "Crear reserva fallida por duración excesiva", op13)

    # ------------------------------------------------------------
    # OPERACIÓN 14: Servicio no disponible.
    # ------------------------------------------------------------
    def op14():
        servicios[1].cambiar_disponibilidad(False)
        reserva = Reserva("R003", clientes[0], servicios[1], duracion=2)
        reservas.append(reserva)
        return reserva.mostrar_informacion()

    ejecutar_operacion(14, "Crear reserva sobre servicio no disponible", op14)

    # ------------------------------------------------------------
    # OPERACIÓN 15: Procesar una reserva sin confirmar.
    # ------------------------------------------------------------
    def op15():
        reserva = Reserva("R004", clientes[0], servicios[2], duracion=2)
        reservas.append(reserva)
        costo = reserva.procesar()
        return f"Costo final: ${costo:,.0f}"

    ejecutar_operacion(15, "Procesar reserva sin confirmarla", op15)

    # ------------------------------------------------------------
    # OPERACIÓN 16: .
    # ------------------------------------------------------------
def aporte_colaborativo_pendiente(clientes, servicios, reservas):

    print("\nAPORTE COLABORATIVO - OPERACIÓN 16")
    print("-" * 70)

    def op16():
        # Crear una nueva reserva válida con asesoría
        reserva = Reserva("R005", clientes[0], servicios[2], duracion=2)
        reservas.append(reserva)

        # Confirmar y procesar
        reserva.confirmar()
        costo = reserva.procesar(descuento=0.05)

        return f"Nueva reserva procesada correctamente. Costo final: ${costo:,.0f}"

    ejecutar_operacion(16, "Crear, confirmar y procesar una nueva reserva válida", op16)

    # Resumen final del sistema.
    print("\n" + "=" * 70)
    print("RESUMEN FINAL")
    print("=" * 70)
    print(f"Clientes válidos registrados: {len(clientes)}")
    print(f"Servicios válidos creados: {len(servicios)}")
    print(f"Reservas creadas correctamente: {len(reservas)}")
    print("Los eventos y errores quedaron registrados en logs/software_fj.log")
    print("=" * 70)
      
  
# ============================================================
# PUNTO DE ENTRADA DEL PROGRAMA
# ============================================================

if __name__ == "__main__":
    main()

