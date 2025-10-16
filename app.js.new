// Main application class to handle placeholder replacement functionality
class PlaceholderReplacer {
    constructor() {
        // Initialize DOM elements
        this.inputTextarea = document.getElementById('input-text');
        this.previewContent = document.getElementById('preview-content');
        this.formContainer = document.getElementById('form-container');
        this.outputContainer = document.getElementById('output-container');
        this.initialEmptyState = document.getElementById('initial-empty-state');
        this.noPlaceholdersState = document.getElementById('no-placeholders-state');
        this.formFields = document.getElementById('form-fields');
        this.placeholderForm = document.getElementById('placeholder-form');
        this.outputContent = document.getElementById('output-content');
        this.copyBtn = document.getElementById('copy-btn');
        this.resetBtn = document.getElementById('reset-btn');
        
        // Template buttons
        this.emailTemplateBtn = document.getElementById('emailTemplate');
        this.webTemplateBtn = document.getElementById('webTemplate');
        this.codeTemplateBtn = document.getElementById('codeTemplate');

        // Define templates
        this.templates = {
            email: `Hello <name>,

Thank you for signing up for <service_name>!

Your account details:
- Username: <username>
- Email: <email>
- Plan: <plan_type>

Welcome to <company_name>!

Best regards,
The <team_name> Team`,
            web: `<!DOCTYPE html>
<html>
<head>
    <title><page_title></title>
</head>
<body>
    <h1>Welcome to <website_name></h1>
    <p>Hello <visitor_name>, this is <description>.</p>
    <p>Contact us at <contact_email></p>
</body>
</html>`,
            code: `function <function_name>(<parameters>) {
    console.log('Processing <action> for <entity>');
    
    const <variable_name> = <default_value>;
    
    return {
        status: '<status>',
        message: '<message>',
        data: <variable_name>
    };
}`
        };
        
        this.placeholders = [];
        this.currentValues = {};
        this.currentInputText = '';
        
        this.init();
    }
    
    init() {
        // Event listeners
        this.inputTextarea.addEventListener('input', () => this.handleInputChange());
        this.placeholderForm.addEventListener('submit', (e) => this.handleFormSubmit(e));
        this.copyBtn.addEventListener('click', () => this.copyToClipboard());
        this.resetBtn.addEventListener('click', () => this.resetForm());
        
        // Template button listeners
        if (this.emailTemplateBtn) {
            this.emailTemplateBtn.addEventListener('click', () => {
                console.log('Email template clicked');
                this.loadTemplate('email');
            });
        }
        if (this.webTemplateBtn) {
            this.webTemplateBtn.addEventListener('click', () => {
                console.log('Web template clicked');
                this.loadTemplate('web');
            });
        }
        if (this.codeTemplateBtn) {
            this.codeTemplateBtn.addEventListener('click', () => {
                console.log('Code template clicked');
                this.loadTemplate('code');
            });
        }
        
        // Initial state - show the initial empty state
        this.showInitialEmptyState();
        
        console.log('PlaceholderReplacer initialized');
    }
    
    handleInputChange() {
        const inputText = this.inputTextarea.value;
        this.currentInputText = inputText;
        
        console.log('Input changed:', inputText);
        
        // Update preview first
        this.updatePreview(inputText);
        
        // Check if input is empty
        if (!inputText.trim()) {
            console.log('Input is empty, showing initial empty state');
            this.showInitialEmptyState();
            return;
        }
        
        // Parse placeholders
        this.placeholders = this.parsePlaceholders(inputText);
        console.log('Found placeholders:', this.placeholders);
        
        // Determine which state to show based on placeholder presence
        if (this.placeholders.length > 0) {
            console.log('Placeholders found, generating form');
            this.generateForm();
            this.showFormContainer();
        } else {
            console.log('No placeholders found, showing no placeholders state');
            this.showNoPlaceholdersState();
        }
    }
    
    parsePlaceholders(text) {
        console.log('Parsing text for placeholders:', text);
        
        // Find all patterns like <placeholder_name>
        const regex = /<([^<>]+)>/g;
        const matches = [];
        const uniquePlaceholders = new Set();
        
        let match;
        while ((match = regex.exec(text)) !== null) {
            const placeholder = match[1].trim();
            console.log('Found placeholder match:', placeholder);
            
            if (placeholder && !uniquePlaceholders.has(placeholder)) {
                uniquePlaceholders.add(placeholder);
                matches.push(placeholder);
            }
        }
        
        console.log('Unique placeholders found:', matches);
        return matches;
    }
    
    updatePreview(text) {
        if (!text.trim()) {
            this.previewContent.innerHTML = '<p class="preview-placeholder-text">Enter text above to see preview with highlighted placeholders...</p>';
            return;
        }
        
        // Escape HTML first, then highlight placeholders
        let highlightedText = this.escapeHtml(text);
        
        // Replace each placeholder with highlighted version
        this.placeholders.forEach(placeholder => {
            const escapedPlaceholder = this.escapeRegex(placeholder);
            const regex = new RegExp(`&lt;${escapedPlaceholder}&gt;`, 'g');
            highlightedText = highlightedText.replace(regex, 
                `<span class="placeholder-highlight">&lt;${placeholder}&gt;</span>`
            );
        });
        
        this.previewContent.innerHTML = highlightedText;
    }
    
    loadTemplate(type) {
        console.log('Loading template:', type);
        if (this.templates && this.templates[type]) {
            console.log('Setting template content');
            this.inputTextarea.value = this.templates[type];
            this.handleInputChange();
        }
    }
    
    escapeHtml(text) {
        const div = document.createElement('div');
        div.textContent = text;
        return div.innerHTML;
    }
    
    escapeRegex(string) {
        return string.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
    }
    
    generateForm() {
        console.log('Generating form for placeholders:', this.placeholders);
        
        // Clear existing form fields
        this.formFields.innerHTML = '';
        
        // Create form fields for each placeholder
        this.placeholders.forEach((placeholder, index) => {
            console.log(`Creating field for placeholder: ${placeholder}`);
            
            const fieldDiv = document.createElement('div');
            fieldDiv.className = 'form-field';
            
            const label = document.createElement('label');
            label.textContent = this.formatPlaceholderLabel(placeholder);
            label.setAttribute('for', `field-${index}`);
            
            const input = document.createElement('input');
            input.type = 'text';
            input.id = `field-${index}`;
            input.name = placeholder;
            input.className = 'form-control';
            input.placeholder = `Enter value for ${placeholder}`;
            input.value = this.currentValues[placeholder] || '';
            input.required = true;
            
            // Store current values as user types
            input.addEventListener('input', () => {
                this.currentValues[placeholder] = input.value;
                console.log('Updated value for', placeholder, ':', input.value);
            });
            
            fieldDiv.appendChild(label);
            fieldDiv.appendChild(input);
            this.formFields.appendChild(fieldDiv);
        });
        
        console.log('Form generated with', this.placeholders.length, 'fields');
    }
    
    formatPlaceholderLabel(placeholder) {
        return placeholder
            .replace(/_/g, ' ')
            .replace(/([A-Z])/g, ' $1')
            .toLowerCase()
            .replace(/^\w/, c => c.toUpperCase())
            .trim();
    }
    
    handleFormSubmit(e) {
        e.preventDefault();
        console.log('Form submitted');
        
        // Get form data
        const formData = new FormData(this.placeholderForm);
        const values = {};
        
        for (let [key, value] of formData.entries()) {
            values[key] = value.trim();
        }
        
        console.log('Form values:', values);
        
        // Validate that all fields are filled
        let allFilled = true;
        for (let placeholder of this.placeholders) {
            if (!values[placeholder]) {
                allFilled = false;
                break;
            }
        }
        
        if (!allFilled) {
            alert('Please fill in all placeholder values before generating output.');
            return;
        }
        
        // Store values for potential reset
        this.currentValues = {...values};
        
        // Generate output with replacements
        const output = this.generateOutput(this.currentInputText, values);
        console.log('Generated output:', output);
        
        // Show output
        this.showOutput(output);
    }
    
    generateOutput(inputText, values) {
        let output = inputText;
        
        // Replace each placeholder with its corresponding value
        this.placeholders.forEach(placeholder => {
            const value = values[placeholder] || `<${placeholder}>`;
            const regex = new RegExp(`<${this.escapeRegex(placeholder)}>`, 'g');
            output = output.replace(regex, value);
        });
        
        return output;
    }
    
    showOutput(output) {
        console.log('Showing output:', output);
        this.outputContent.textContent = output;
        
        // Hide all other containers and show output
        this.hideAllStates();
        this.outputContainer.classList.remove('hidden');
        
        console.log('Output container should be visible now');
    }
    
    copyToClipboard() {
        const text = this.outputContent.textContent;
        console.log('Copying to clipboard:', text);
        
        if (navigator.clipboard && navigator.clipboard.writeText) {
            navigator.clipboard.writeText(text).then(() => {
                this.showCopySuccess();
            }).catch(() => {
                this.fallbackCopy(text);
            });
        } else {
            this.fallbackCopy(text);
        }
    }
    
    fallbackCopy(text) {
        const textarea = document.createElement('textarea');
        textarea.value = text;
        textarea.style.position = 'fixed';
        textarea.style.opacity = '0';
        document.body.appendChild(textarea);
        
        try {
            textarea.select();
            document.execCommand('copy');
            this.showCopySuccess();
        } catch (err) {
            console.error('Copy failed:', err);
            alert('Copy failed. Please select the text manually.');
        }
        
        document.body.removeChild(textarea);
    }
    
    showCopySuccess() {
        const originalText = this.copyBtn.textContent;
        this.copyBtn.textContent = 'Copied!';
        this.copyBtn.classList.add('copy-success');
        
        setTimeout(() => {
            this.copyBtn.textContent = originalText;
            this.copyBtn.classList.remove('copy-success');
        }, 2000);
    }
    
    resetForm() {
        console.log('Resetting form');
        
        // Hide output and show appropriate state based on current input
        this.outputContainer.classList.add('hidden');
        
        if (this.placeholders.length > 0) {
            this.showFormContainer();
        } else {
            this.showInitialEmptyState();
        }
    }
    
    // UI State Management
    hideAllStates() {
        console.log('Hiding all states');
        this.formContainer.classList.add('hidden');
        this.outputContainer.classList.add('hidden');
        this.initialEmptyState.classList.add('hidden');
        this.noPlaceholdersState.classList.add('hidden');
    }
    
    showFormContainer() {
        console.log('Showing form container');
        this.hideAllStates();
        this.formContainer.classList.remove('hidden');
    }
    
    showInitialEmptyState() {
        console.log('Showing initial empty state');
        this.hideAllStates();
        this.initialEmptyState.classList.remove('hidden');
    }
    
    showNoPlaceholdersState() {
        console.log('Showing no placeholders state');
        this.hideAllStates();
        this.noPlaceholdersState.classList.remove('hidden');
    }
}

// Initialize the application when DOM is loaded
document.addEventListener('DOMContentLoaded', () => {
    console.log('DOM loaded, initializing app');
    new PlaceholderReplacer();
});
