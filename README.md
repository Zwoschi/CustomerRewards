# CustomerRewards
Microsoft Dynamics 365 Business Central "Customer Rewards" Extension

# Unit Tests

# Erstellen von Erweiterungen

# Übersicht

https://docs.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/devenv-extension-advanced-example

- Table 50100 &quot;Reward Level&quot;
- Table 50101 &quot;Activation Code Information&quot;
- Table 50102 &quot;Customer Rewards Mgt. Setup&quot;
- Tableextension 50100 &quot;CustomerTable Ext.&quot; extends Customer

- Page 50100 &quot;Customer Rewards Wizard&quot;
- Page 50101 &quot;Rewards Level List&quot;
- Pageextension 50100 &quot;Customer Card Ext.&quot; extends &quot;Customer Card&quot;
- Pageextension 50101 &quot;Customer List Ext.&quot; extends &quot;Customer List&quot;

- Codeunit 50100 &quot;Customer Rewards Install Logic&quot;
- Codeunit 50101 &quot;Customer Rewards Ext. Mgt.&quot;

#  „Customer Rewards&quot; Erweiterungsübersicht

- Erweiterung ermöglicht das Setzen von „Reward Levels&quot;
- Jedes Level hat eine zugehörige „minimum number of reward point&quot;
- Jedem Kunden kann so ein Level zugeordnet werden
- Ohne Zuordnung ist das Level auf „NONE&quot; gesetzt
- Bei jeder Bestellung erhält der Kunde „reward points&quot;
- Zu Beginn der Nutzung der Erweiterung muss der Benutzer im **Customer Rewards Assisted Setup Wizard**  **den Nutzungsbedingungen zustimmen und einen Aktivierungscode eingeben**

# Reward Level - Tabellenobjekte

## Table 50100 &quot;Reward Level&quot;

-beinhaltet „reward level&quot; Informationen, die vom Benutzer angelegt werden können

- 2 neue Felder: **Level** und **Minimum Reward Points**

## Table 50101 &quot;Activation Code Information&quot;

- Beinhaltet Aktivierungsinformationen der Erweiterung
- Besteht aus 3 Feldern: **ActivationCode** , **Date Activated** und **Expiration Date**

## Table 50102 &quot;Customer Rewards Mgt. Setup&quot;

- Beinhaltet Informationen über die Codeunit, welche die Events bei der Ausführung steuert
- Dies ermöglicht es Ereignisse im Beispieltest zu simulieren
- Besteht aus 3 Feldern: **Primary Key** und **Customer Rewards Ext. Mgt. Codeunit ID**

# Customer Rewards – Tabellenerweiterungsobjekte

## Tableextension 50100 &quot;CustomerTable Ext.&quot; extends Customer

- Da Die **Customer** -Tabelle Teil des Dynamics 365 BS Services ist kann diese (wie viele Weitere) nicht modifiziert werden
- Fügt ein neues Feld hinzu: **RewardPoints**

# Customer Rewards Wizard - Pageobjekte

## Page 50100 &quot;Customer Rewards Wizard&quot;

- Ermöglicht dem Benutzer die Nutzungsbedingungen der Erweiterung zuzustimmen und die Erweiterungen zu aktivieren
- Enthält 3 Schritte:
  - Willkommen – Checkbox zum Zustimmen der Nutzungsbedingungen
  - Aktivierung – TextBox zum eintragen des Aktivierungscodes\*
  - Beenden

\*ein gültiger Aktivierungscode besteht aus beliebigen 14 alphanumerischen Zeichen

## Page 50101 &quot;Rewards Level List&quot;

- Ermöglicht dem Benutzer das Anschauen, bearbeiten und hinzufügen von „reward levels&quot; und „minimum required points&quot;

# „Customer Rewards&quot; - Pageerweiterungsobjekte

## Pageextension 50100 &quot;Customer Card Ext.&quot; extends &quot;Customer Card&quot;

- Eine Pageextension ermöglicht das Hinzufügen von neuen Funktionen zu einer Page, die Teil des Dynamics 365 BS Services ist
- Hinzufügen von 2 neuen Feldern zur **Customer Card** : **RewardLevel** und **RewardPoints**
- Felder befinden sich nach dem **Name** Fieldcontrol auf der Seite
- Hinzugefügt in der Layout Section

## Pageextension 50101 &quot;Customer List Ext.&quot; extends &quot;Customer List&quot;

- Erweitert die ** Customer List** – Page um ein Control - **Reward Levels** in der **Customer** -Gruppe

# „Customer Rewards&quot; – Codeunitobjekte

## Codeunit 50100 &quot;Customer Rewards Install Logic&quot;

- Initialisiert Standard Codeunit zum Steuern von Events
- Subtype = Install;
- **OnInstallAppPerCompany**  **-**** Trigger wird ausgeführt, wenn die Erweiterung zum Ersten Mal installiert wird**

## Codeunit 50101 &quot;Customer Rewards Ext. Mgt.&quot;

- Umfasst einen Großteil der Logik und Funktionen, welche für die „Customer Rewards&quot; Erweiterung notwendig sind
- Beinhaltet Beispiele zur Verwendung von Events und wie man auf spezifische Aktionen reagiert, sowie Verhaltensweisen, die innerhalb der Erweiterung auftreten können



- Definition einer „Event Publisher&quot;-Methode **OnGetActivationCodeStatusFromServer**** , die den vom Benutzer eingegebenen Aktivierungscode akzeptiert**
- **„Subscriber&quot;-Methode**  **OnGetActivationCodeStatusFromServerSubscriber**  **wartet und steuert das Event**
- **Der**  **ActivateCustomerRewards**  **wird ausgeführt**
- ** Die**  **OnGetActivationCodeStatusFromServer**  **wird aufgerufen**
- **Weil die**  **EventSubscriberInstance**  **Berechtigung**** der Codeunit standartmäßig auf **** Static-Automatic **** gesetzt ist, wird der **** OnGetActivationCodeStatusFromServerSubscriber ****-Trigger aufgerufen**
- **Es wird geschaut, ob die aktuell aufgerufene Codeunit das Event steuern kann, wenn nicht, wird der**  **GetHttpResponse**** -Trigger aufgerufen, um den Aktivierungscode zu validieren**

**Die „Customer Rewards&quot; Beispielerweiterung ist an diesem Punkt fertig!**

# Testen von Erweiterungen

# Übersicht

[https://docs.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/devenv-extension-advanced-example-test](https://docs.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/devenv-extension-advanced-example-test)

- Codeunit 50102 MockCustomerRewardsExtMgt
- Codeunit 50103 &quot;Customer Rewards Test&quot;

# Identifikation der zu testenden Bereiche

- Vergewissere dich, dass der Test alle Einstellungs- und Verwendungsmöglichkeiten abdeckt
- Dies Umfasst: Assisted Setups, pages, fields, actions, events und weitere Controls und Objekte, die in einer Erweiterung verwendet werden können
- Verwende die CRONUS         Demo
- Erstelle Tests, die Benutzer ohne SUPER-Berechtigung einbeziehen
- Tests sollten keine externen Services aufrufen

Der Beispiel-Test beinhaltet Folgendes:

- Logik in der **Install** Codeunit
- **Assisted Setup - Customer Rewards Wizard** page

-  Es wird verifiziert, dass der Wizard erwartungsgemäß läuft. Das „Assisted Setup&quot; beinhaltet Code, der Zugriff auf einen externen Server oder API simuliert

- **Reward Level** page

- Es wird verifiziert, dass die Page erwartungsgemäß läuft, wenn sie geöffnet wird – unabhängig davon ob die „Customer Rewards&quot; Funktion aktiv ist oder nicht

- **Customer List** page

- Es wird geprüft, ob die **Reward Levels** Aktion auf der Page existiert und ob sie erwartungsgemäß funktioniert – unabhängig davon, ob diese Funktion aktiv

- **Customer Card** page

- Prüft ob die Page die neuen Felder **Reward Level** und **Rewards Points** hinzugefügt hat

- **New Customer**

- Sollte Null „Reward Points&quot; besitzen und das dementsprechende „Reward Level&quot;

- Verschiedene Szenarien betreffend Kunden und Sales Orders, um zu prüfen ob die **Reward Points** und die „Reward Levels&quot; wie vom Benutzer definiert funktionieren
- Die Tests zeigen ebenfalls, ob die Erweiterungen auch für einen Benutzer ohne SUPER-Berechtigung funktioniert

# Schreiben der Tests

Erstelle ein neues Projekt (CustomerRewardsTest) für den Test. Es ist notwendig die Erweiterung und die Tests durch separate Projekte zu trennen.

- Spezifiziere die Abhängigkeiten (dependencies) zwischen der Erweiterungs- und dem Testprojekt

# Application Toolkit

Das Tool Kit wird verwendet, um die Tests zu automatisieren und auszuführen.  Es enthält:

- Codeunits mit Testfunktionen, um verschiedene Bereiche des Programms zu testen
- Codeunit mit generischen und programmspezifischen Funktionen, um Codeduplikation zu reduzieren
- Objekte zum Ausführen von Tests z.B. der **Test Tool** Page

## Installation des Applikation Test Toolkit

1. Öffne den „NAV Container Helper&quot; promt. Dort befindet sich eine Liste von Funktionen, die im Container ausführbar sind.
2. Starte die **Import-TestToolkitNavContainer -containerName \&lt;name-of-container\&gt;** Funktion mit dem Parameter **-containerName** um das Test Toolkit in die Programmdatenbank zu laden.

Alternativ kann mit der **New-NavContainer** Funktion des NavContainerHelper PowerShell Moduls ein neuer Container in Docker erstellt werden. Der Befehl **-includeTestToolkit** installiert das Application Toolkit während der Erstellung des Containers.

## Beschreibe die Tests

Um relevante Tests zu erstellen ist es möglich Szenarien zu erstellen, die zeigen was getestet werden möchte. Testkriterien können im GIVEN-WHEN-THEN Format geschrieben werden. Durch das hinzufügen solcher Kommentare (Feature, Szenario und GIVEN-WHEN-THEN) wird dem Code Struktur gegeben und die Tests sind lesbarer.

## Übersicht der Tags

##### FEATURE Tag

// [FEATURE] [\&lt;FeatureTag1\&gt;] [\&lt;FeatureTagN\&gt;]

- Repräsentiert den Namen eines Features, eines Programmbereiches, eines Funktionsbereiches oder eines anderen Aspektes der Anwendung
- Die Liste von Tags muss sich in einem Bereich der Lösung des Programms befinden, der mit dem Test verknüpft ist
- Ordne die Tags nach absteigender Priorität, beginne mit den wichtigsten Tag
- Das [FEATURE] Tag kann für eine gesamte Test-Codeunit gesetzt werden. Dies bedeutet, alle Tests innerhalb der Codeunit beziehen sich auf die dort gesetzten Tags

##### SCENARIO Tag

// [SCENARIO \&lt;ScenarioID\&gt;] \&lt;TestDescription\&gt;

- **ScenenarioID** verknüpft ein Arbeitselement für die Funktionalität, z.B., wenn Visual Studio Online oder Team Foundation Server genutzt wird. [SCENARIO 12345] repräsentiert dann ein Arbeitselement mit der ID 12345
- **TestDescriptions** sind eine kurze Beschreibung bezüglich des Zwecks eines Tests

##### GIVEN-WHEN-THEN Tags

###### GIVEN

- Beschreibt einen Schritt zum Einrichten des Tests
- Separate GIVEN sind einem AND vorzuziehen (es können mehrere GIVEN vorkommen)

###### WHEN

- Beschreibt die zu testende Aktion
- Ein Test testet nur eine Sache
- Es sollte nur ein WHEN im Test vorkommen, wenn mehrere WHEN benötigt werden, z.B. durch unterschiedliche Verifizierungsmöglichkeiten sollte der Test gesplittet werden

###### THEN

- Beschreibt was verifiziert wird
- Es kann mehr als ein THEN geben

# „MockCustomerRewardsExtMgt&quot; Codeunitobjekt

## Codeunit 50102 „MockCustomerRewardsExtMgt&quot;

- Beinhaltet den Code, der den Prozess zur Validierung des Aktivierungscodes des „Customer Rewards&quot; simuliert
- Weil im Test kein externer Server augerufen werden kann, wird die Subscriber Methode **MockOnGetActivationCodeStatusFromServerSubscriber**  **Diese steuert das**  **OnGetActivationCodeStatusFromServer**  **Event, wenn in der**  **Customer Rewards Ext. Mgt.**  **Aufgerufen wird**
- **Die**  **EventSubscriberInstance**  **Property für diese Codeunit wird auf**  **Manual**  **gesetzt, so kann kontrolliert werden wann die Subscriber Funktion aufgerufen wird**
- **Die Subscriber Methode soll nur während des Tests ausgeführt werden**
- **Außerdem wird eine Einstellung erstellt, dass die**  **Customer Rewards Ext. Mgt. Codeunit ID** in der **Customer Rewards Mgt. Setup**  **Tabelle**** modifiziert, so dass der **** OnGetActivationCodeStatusFromServerSubscriber **** das **** OnGetActivationCodeStatusFromServer **** Event ignoriert, auch wenn es aufgerufen wird**

# Codeunit-Testobjekte

Eine Test-Codeunit muss den Subtype **Test** besitzen, ebenfalls benötigen Test-Methoden ein [Test] Attribut. Wenn eine Test-Codeunit läuft, führt diese den  **OnRun** -Trigger aus und durchläuft alle Test-Methoden. Der Output einer solchen Methode ist entweder SUCCESS oder FAILURE. Auftretende Fehler werden im „Results Log File&quot; gespeichert. Auch wenn die Durchführung einer Methode fehlschlägt werden die weitern Methoden dennoch ausgeführt.

## Test-Pages

- Simulieren echte Pages, jedoch ohne Benutzeroberfläche
- Werden durch AL Methoden gesteuert (öffnen, schließen, durchführen von Aktionen oder Navigation)

## UI Handlers

- Um Tests zu automatisieren müssen Fälle abgedeckt sein die Benutzerinteraktion benötigen
- Haben den gleichen Ausgangszustand wie die originale Page
- Z.B. eine Test-Methode, die einen **ConfirmHandler** besitzt, übernimmt die Aufgabe der CONFIRM Methode
- Es muss für jede Page ein spezifischer Handler geschrieben werden
- Jede nicht gesteuerte UI in der Test-Methode der Test-Codeunit verursacht einen Fehler des Tests

## ASSERTERROR Statement

- Bei einem Test einer Erweiterung sollte darauf geachtet werden, dass der Code wie erwartet funktioniert – im Falle eines gelungenen Tests, wie auch bei einem fehlerhaften Test
- Dies wird positiv und negativ Test genannt
- Um zu Testen wie eine Erweiterung unter fehlerhaften Umständen reagiert kann das ASSERTERROR Schlüsselwart benutzt werden
- Ein Statement diesem Schlüsselwort läuft nach einem verursachten Error dennoch erfolgreich weiter zum nächsten Statement in der Test-Funktion
- Falls das darauffolgende Statement keinen Fehler verursacht, dann wirft das ASSERTERROR einen Fehler und die Test-Funktion liefert ein FAILURE Resultat

Die Codeunit 50103 **Customer Reward Test**  beinhaltet all diese Tests für die „Customer Rewards&quot; Erweiterung. Für jede Test-Methode gilt folgendes Muster:

- Initialisierung und Erstellen von Testbedingungen
- Aufrufen der zu testenden Business Logic
- Bestätigung das die Business Logic erwartungsgemäß funktioniert

## TestOnInstallLogic Test

- Bestätigt, dass die erstellte Logik in der Install-Codeunit wie erwartet funktioniert
- Zuerst wird die Helper-Methode **Initialize** aufgerufen, diese initialisiert und räumt alle für den Test benötigten Objekte auf
- Verlinkt zudem die **MockCustomerRewardsExtMgt**  **zur Test-Codeunit, so kann jedes aufgerufene Event durch die Subscriber-Methode gesteuert werden**
- **Nachdem wird die**  **SetDefaultCustomerRewardsExtMgtCodeunit**** -Methode ****aufgerufen, diese ist in der Install-Codeunit definiert**
- **Zuletzt wird durch die**  **Assert**** -Codeunit aus dem Application Toolkit, welches die ****Customer Rewards Mgt. Setup**  **beinhaltet**** die erwartete Codeunit ID bestätigt**

## TestCustomerRewardsWizardActivationPageErrorsWhenInvalidActivationCodeEntered Test

- Bezieht sich auf den **Customer Rewards Assisted Setup Guide**  **und bestätigt, dass eine**** Error-Nachricht erscheint, wenn ein ungültiger Aktivierungscode im Wizard eingegeben wird**
- **Zuerst wird**  **Initialize**  **aufgerufen, um den vorherigen Zustand zu bereinigen und die simulierenden Subscriber-Methoden in die Test-Codeunit einzubinden**
- ** Die**  **Library - Lower Permissions**  **Codeunit wird verwendet um die Nutzerberechtigungen, wie ein Nutzer ohne SUPER Berechtigung  **
- **Danach wird die Page**  **Customer Rewards Wizard**  **simuliert und der eingegebene Aktivierungscode wird aktiviert**
- **Zuletzt wird bestätigt das eine Error-Nachricht angezeigt wir, weil die Bestätigung der Aktivierung fehlschlägt. Wenn kein Anderer Fehler angezeigt wird kann daraus geschlossen werden, dass Funktionalität in diesem Test auch ohne SUPER Berechtigung gewährleistet ist**

## TestRewardLevelsActionExistsOnCustomerListPage Test

- Bestätigt das die neue **Reward Levels** Aktion auf der Customer List Page existiert

## TestCustomerHasBronzeRewardLevelAfterPostedSalesOrders Test

- Berücksichtigt die Interaktion zwischen Customers, Sales Orders und Reward Levels
- Bestätigt wenn zwei Sales Orders bei einem neuen Kunden getätigt werden, der Kunde bekommt zwei „Reward Points&quot;
- Zuerst wird der Test durch **Initialize** initialisiert
- Die Erweiterung ist aktiv und das BRONZE „Reward Level&quot; für zwei Punkte wird in der **Reward Level** Tabelle eingestellt
- Danach wird ein neuer Kunde mit Hilfe der **LibrarySales** -Codeunit erstellt
- Ebenfalls wird die Codeunit verwendet um zwei Sales Orders zu erstellen und aufzugeben für den zuvor erstellten Kunden
- Zuletzt wird geprüft, dass der Kunde die richtigen Reward Points und Level bekommen hat

# Ausführen der Tests

1. Öffne die **Test Tool** Page (130401)
2. Wähle **Get Test Codeunits** , dann wähle **Select Test Codeunits**
3. Wähle die Test-Codeunit und bestätige mit OK
4. Nun wähle **Run** oder **Run Selected** , um alle oder nur ausgewählte Tests der gewählten Codeunit auszuführen. In der Spalte **Result** werden die Ergebnisse der Tests gespeichert.
