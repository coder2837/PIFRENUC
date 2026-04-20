import customtkinter as ctk
import os
import subprocess

class PifrenucTerminal(ctk.CTk):
    def __init__(self):
        super().__init__()
        self.title("PIFRENUC 0.1")
        self.geometry("600x400")

        # 1. The Terminal Display
        self.terminal_display = ctk.CTkTextbox(self, width=560, height=250)
        self.terminal_display.pack(pady=10)
        self.terminal_display.insert("0.0", "--- PIFRENUC 0.1 ---\nType: echo(\"text\") or GVP\n")

        # 2. The Input Area
        self.input_box = ctk.CTkEntry(self, width=450, placeholder_text='Try: echo("Hello World")')
        self.input_box.pack(side="left", padx=20, pady=10)
        self.input_box.bind("<Return>", lambda e: self.process_command())

        # 3. Execute Button
        self.run_btn = ctk.CTkButton(self, text="Execute", width=80, command=self.process_command)
        self.run_btn.pack(side="left", pady=10)

    def process_command(self):
        raw_input = self.input_box.get().strip()
        self.input_box.delete(0, 'end')
        
        output = ""

        # --- PYTHON STYLE ECHO: echo("text") ---
        if raw_input.startswith('echo("') and raw_input.endswith('")'):
            # Extract text between echo(" and ")
            content = raw_input[6:-2]
            output = f"> {content}\n"

        # --- GVP COMMAND ---
        elif raw_input.lower() == "gvp":
            if os.path.exists("engine.exe"):
                output = " GVP SUCCESS.( or import) 'engine.exe' found in PIFRENUC files.\n"
            else:
                output = " GVP( or import) FAIL. Engine missing. has not been found in PIFRENUC files.\n"

        # --- ERROR HANDLING ---
        else:
            output = f"ERROR: Invalid syntax. Use echo(\"text\") or GVP.\n"

        self.terminal_display.insert("end", output)
        self.terminal_display.see("end")

if __name__ == "__main__":
    app = PifrenucTerminal()
    app.mainloop()
